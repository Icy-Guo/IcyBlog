---
title: Leetcode Matrix
date: 2024-12-24 22:41:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

### **Description**

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`s.

You must do it in place.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
- **Output**: `[[1,0,1],[0,0,0],[1,0,1]]`

---

**Example 2:**

- **Input**: `matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
- **Output**: `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

---

</details>

### **Solution**

#### **Approach 1:** 标记数组

**Idea:** 使用两个布尔数组分别记录矩阵的**行**和**列**是否需要被置零。

**Complexity:** Time: _O(m \* n)_, Space: _O(m + n)_

```python
class Solution(object):
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        m = len(matrix)
        n = len(matrix[0])
        rows = [0] * m
        columns = [0] * n

        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    rows[i] = True
                    columns[j] = True

        for i in range(m):
            for j in range(n):
                if rows[i] or columns[j]:
                    matrix[i][j] = 0

        return matrix
```

#### **Approach 2:** 使用矩阵的第一行和第一列来标记

**Idea:** 使用矩阵的**第一行**和**第一列**来标记需要被置零的行和列，可以达到 _O(1)_ 的额外空间。但这样会导致原数组的第一行和第一列被修改，无法记录它们是否原本包含 `0`。因此我们需要额外使用两个标记变量分别记录第一行和第一列是否原本包含 `0`。

**Complexity:** Time: _O(m \* n)_, Space: _O(1)_

```python
class Solution:
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        m, n = len(matrix), len(matrix[0])
        flag_col0 = any(matrix[i][0] == 0 for i in range(m))
        flag_row0 = any(matrix[0][j] == 0 for j in range(n))

        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = matrix[0][j] = 0

        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0

        if flag_col0:
            for i in range(m):
                matrix[i][0] = 0

        if flag_row0:
            for j in range(n):
                matrix[0][j] = 0
```

## [54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

### **Description**

Given an `m x n` matrix, return all elements of the matrix in spiral order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
- **Output**: `[1,2,3,6,9,8,7,4,5]`

---

**Example 2:**

- **Input**: `matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
- **Output**: `[1,2,3,4,8,12,11,10,9,5,6,7]`

---

</details>

### **Solution**

**Idea:** 可以将矩阵看成若干层，首先输出最外层的元素，其次输出次外层的元素，直到输出最内层的元素。

**Complexity:** Time: _O(m \* n)_, Space: _O(1)_

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        rows, columns = len(matrix), len(matrix[0])
        left, right, top, bottom = 0, columns - 1, 0, rows - 1
        ans = list()

        while left <= right and top <= bottom:
            for j in range(left, right + 1):
                ans.append(matrix[top][j])
            for i in range(top + 1, bottom + 1):
                ans.append(matrix[i][right])
            if left < right and top < bottom:
                for j in range (right - 1, left - 1, -1):
                    ans.append(matrix[bottom][j])
                for i in range (bottom - 1, top, -1):
                    ans.append(matrix[i][left])
            left, right, top, bottom = left + 1, right - 1, top + 1, bottom - 1

        return ans
```

## [48. Rotate Image](https://leetcode.com/problems/rotate-image/)

### **Description**

You are given an `n x n` 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
- **Output**: `[[7,4,1],[8,5,2],[9,6,3]]`

---

**Example 2:**

- **Input**: `matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]`
- **Output**: `[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]`

---

</details>

### **Solution**

**Idea:** 这道题直觉上很容易想到用一个额外的矩阵来存储旋转后的结果，但是题目要求我们直接在原矩阵上进行修改。经过观察发现：当一个元素进行旋转时，经过 4 次旋转后，它将回到原来的位置。因此，我们可以通过一次性交换 4 个元素的位置来实现旋转，这样就可以解决覆盖原数组的问题。

当 `n` 为偶数时，我们需要枚举 `(n ^ 2) / 4 = (n / 2) * (n / 2)` 个位置。

<img src="/img/leetcode48-2.png" width="200" alt="n 为偶数时">

当 `n` 为奇数时，最中间的元素不需要旋转，因此我们需要枚举 `(n ^ 2 - 1) / 4 = ((n - 1) / 2) * ((n + 1) / 2)` 个位置。

<img src="/img/leetcode48-1.png" width="200" alt="n 为奇数时">

**Complexity:** Time: _O(n ^ 2)_, Space: _O(1)_

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        n = len(matrix)

        for i in range(n // 2):
            for j in range((n + 1) // 2):
                matrix[i][j], matrix[n - j - 1][i], matrix[n - i - 1][n - j - 1], matrix[j][n - i - 1] = matrix[n - j - 1][i], matrix[n - i - 1][n - j - 1], matrix[j][n - i - 1], matrix[i][j]

        return matrix
```

## [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

### **Description**

Write an efficient algorithm that searches for a value target in an `m x n` integer matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending order from left to right.
- Integers in each column are sorted in ascending order from top to bottom.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5`
- **Output**: `true`

---

**Example 2:**

- **Input**: `matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 从矩阵的 **右上角** 开始搜索，如果当前元素大于目标值，则向左移动，如果当前元素小于目标值，则向下移动。选择右上角作为起点是因为该位置的元素是 **该行最大的，是该列最小的**，不会漏掉某个元素。同样也可以选择左下角作为起点。

**Complexity:** Time: _O(m + n)_, Space: _O(1)_

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        rows = len(matrix)
        cols = len(matrix[0])
        row = 0
        col = cols - 1

        while row < rows and col >= 0:
            if matrix[row][col] == target:
                return True
            if matrix[row][col] > target:
                col -= 1
            if matrix[row][col] < target:
                row += 1
        
        return False
```

---

`Top Interview 150 补充`

---

## [36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

### **Description**

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

- Each row must contain the digits `1-9` without repetition.
- Each column must contain the digits `1-9` without repetition.
- Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `board = [["8","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用三个矩阵分别记录 **行**、**列**和 **3x3 子数独**中每个数字出现的次数。

**Complexity:** Time: _O(1)_, Space: _O(1)_ (因为 board 是 9x9 的固定大小)

```python
class Solution(object):
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        rows = [[0] * 9 for _ in range(9)]
        columns = [[0] * 9 for _ in range(9)]
        boxes = [[[0] * 9 for _ in range(3)] for _ in range(3)]

        for i in range(9):
            for j in range(9):
                num = board[i][j]
                if num != '.':
                    num = int(num) - 1
                    rows[i][num] += 1
                    columns[j][num] += 1
                    boxes[i // 3][j // 3][num] += 1
                    if rows[i][num] > 1 or columns[j][num] > 1 or boxes[i // 3][j // 3][num] > 1:
                        return False

        return True
```

## [73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

### **Description**

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`s.

You must do it in place.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
- **Output**: `[[1,0,1],[0,0,0],[1,0,1]]`

---

**Example 2:**

- **Input**: `matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
- **Output**: `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

---

</details>

### **Solution**

#### **Approach 1:** 标记数组

**Idea:** 使用两个布尔数组分别记录矩阵的**行**和**列**是否需要被置零。

**Complexity:** Time: _O(m \* n)_, Space: _O(m + n)_

```python
class Solution(object):
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        m = len(matrix)
        n = len(matrix[0])
        rows = [0] * m
        columns = [0] * n

        for i in range(m):
            for j in range(n):
                if matrix[i][j] == 0:
                    rows[i] = True
                    columns[j] = True

        for i in range(m):
            for j in range(n):
                if rows[i] or columns[j]:
                    matrix[i][j] = 0

        return matrix
```

#### **Approach 2:** 使用矩阵的第一行和第一列来标记

**Idea:** 使用矩阵的**第一行**和**第一列**来标记需要被置零的行和列，可以达到 _O(1)_ 的额外空间。但这样会导致原数组的第一行和第一列被修改，无法记录它们是否原本包含 `0`。因此我们需要额外使用两个标记变量分别记录第一行和第一列是否原本包含 `0`。

**Complexity:** Time: _O(m \* n)_, Space: _O(1)_

```python
class Solution:
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        m, n = len(matrix), len(matrix[0])
        flag_col0 = any(matrix[i][0] == 0 for i in range(m))
        flag_row0 = any(matrix[0][j] == 0 for j in range(n))

        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][j] == 0:
                    matrix[i][0] = matrix[0][j] = 0

        for i in range(1, m):
            for j in range(1, n):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0

        if flag_col0:
            for i in range(m):
                matrix[i][0] = 0

        if flag_row0:
            for j in range(n):
                matrix[0][j] = 0
```

## [289. Game of Life](https://leetcode.com/problems/game-of-life/)

### **Description**

According to [Wikipedia's article](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life): "The Game of Life, also known simply as Life, is a cellular automaton devised by the mathematician John Conway."

Given a board with `m x n` cells, each cell has an initial state: `1` which means live (or `0` which means dead). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules:

- Any live cell with fewer than two live neighbors dies, as if caused by under-population.
- Any live cell with two or three live neighbors lives on to the next generation.
- Any live cell with more than three live neighbors dies, as if by over-population.
- Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

The board is made up of `0`s and `1`s, and your function should return the next state of the board after applying the above rules.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]`
- **Output**: `[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]`

---

**Example 2:**

- **Input**: `board = [[1,1],[1,0]]`
- **Output**: `[[1,1],[1,1]]`

---

</details>

### **Solution**

#### **Approach 1:** 使用额外空间

**Idea:** 复制一份原矩阵，根据复制矩阵中邻居细胞的状态来更新原矩阵中的细胞状态。

**Complexity:** Time: _O(m \* n)_, Space: _O(m \* n)_

```python
class Solution(object):
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        rows = len(board)
        cols = len(board[0])
        neighbors = [(1,0), (1,-1), (0,-1), (-1,-1), (-1,0), (-1,1), (0,1), (1,1)]
        copy_board = [[board[row][col] for col in range(cols)] for row in range(rows)]

        for row in range(rows):
            for col in range(cols):
                live_neighbors = 0
                for neighbor in neighbors:
                    r = (row + neighbor[0])
                    c = (col + neighbor[1])
                    if (r < rows and r >= 0) and (c < cols and c >= 0) and (copy_board[r][c] == 1):
                        live_neighbors += 1
                # Rule 1 & 3
                if copy_board[row][col] == 1 and (live_neighbors < 2 or live_neighbors > 3):
                    board[row][col] = 0
                # Rule 4
                if copy_board[row][col] == 0 and live_neighbors == 3:
                    board[row][col] = 1
```

#### **Approach 2:** 使用额外状态

**Idea:** 使用两个额外状态来表示细胞的变化，这样可以解决原矩阵被覆盖的问题，并且不需要额外空间。

- 0: 死细胞
- 1: 活细胞
- **2: 死细胞变为活细胞**
- **3: 活细胞变为死细胞**

**Complexity:** Time: _O(m \* n)_, Space: _O(1)_

```python
class Solution(object):
    def gameOfLife(self, board):
        """
        :type board: List[List[int]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        rows = len(board)
        cols = len(board[0])
        neighbors = [(1, 0), (1, -1), (0, -1), (-1, -1), (-1, 0), (-1, 1), (0, 1), (1, 1)]

        for row in range(rows):
            for col in range(cols):
                live_neighbors = 0
                for neighbor in neighbors:
                    r = row + neighbor[0]
                    c = col + neighbor[1]
                    if 0 <= r < rows and 0 <= c < cols:
                        if board[r][c] == 1 or board[r][c] == 3:
                            live_neighbors += 1
                if board[row][col] == 1:
                    # Rule 1 & 3
                    if live_neighbors < 2 or live_neighbors > 3:
                        board[row][col] = 3
                elif board[row][col] == 0:
                    # Rule 4
                    if live_neighbors == 3:
                        board[row][col] = 2

        for row in range(rows):
            for col in range(cols):
                if board[row][col] == 2:
                    board[row][col] = 1
                elif board[row][col] == 3:
                    board[row][col] = 0
```
