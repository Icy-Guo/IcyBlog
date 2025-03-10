---
title: Leetcode Graph
date: 2025-01-20 22:41:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

### **Description**

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `grid = [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]`
- **Output**: `1`

---

**Example 2:**

- **Input**: `grid = [["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]`
- **Output**: `3`

---

</details>

### **Solution**

**Idea:** 用深度优先搜索（DFS）来遍历网格，并标记访问过的岛屿。每当遇到 `'1'` 时，`count` 加一，并调用 `dfs` 函数递归将该岛屿的所有相邻陆地标记为 `'2'`，以避免重复访问。

**Complexity:** Time: _O(m * n)_, Space: _O(m * n)_

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0
        rows, cols = len(grid), len(grid[0])
        count = 0

        def dfs(row, col):
            if row < 0 or row >= rows or col < 0 or col >= cols or grid[row][col] != '1':
                return
            grid[row][col] = '2'
            dfs(row - 1, col)
            dfs(row + 1, col)
            dfs(row, col - 1)
            dfs(row, col + 1)

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == '1':
                    count += 1
                    dfs(row, col)
        
        return count
```

## [994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### **Description**

You are given an `m x n` grid where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange,
- `2` representing a rotten orange.

Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return `-1`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `grid = [[2,1,1],[1,1,0],[0,1,1]]`
- **Output**: `4`

---

**Example 2:**

- **Input**: `grid = [[2,1,1],[0,1,1],[1,0,1]]`
- **Output**: `-1`

---

</details>

### **Solution**

**Idea:** 使用广度优先搜索（BFS）来模拟腐烂过程。首先，遍历网格，记录新鲜橘子的数量和腐烂橘子的位置。然后，使用 BFS 从腐烂橘子开始，逐分钟感染相邻的新鲜橘子，直到所有新鲜橘子都被感染或无法继续感染。

**为什么不用 DFS？** 

题目要求所有腐烂橘子同时向四周扩散，即 **多个起点同时传播**。DFS 是单一路径递归，不能高效处理这种情况。

- BFS 可以逐层传播，记录层数（即分钟数）。
- DFS 递归深入到底再回溯，不适合处理“同时发生”的事件。


**Complexity:** Time: _O(m * n)_, Space: _O(m * n)_

```python
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        if not grid or not grid[0]:
            return -1
        
        rows, cols = len(grid), len(grid[0])
        queue = deque()
        mins = 0
        fresh_count = 0
        dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    fresh_count += 1
                if grid[r][c] == 2:
                    queue.append((r, c))
        
        if fresh_count == 0:
            return mins
        
        while queue:
            mins += 1
            for _ in range(len(queue)):
                x, y = queue.popleft()
                if 0 <= x + 1 < rows and 0 <= y < cols and grid[x + 1][y] == 1:
                    grid[x + 1][y] = 2
                    queue.append((x + 1, y))
                    fresh_count -= 1
                if 0 <= x - 1 < rows and 0 <= y < cols and grid[x - 1][y] == 1:
                    grid[x - 1][y] = 2
                    queue.append((x - 1, y))
                    fresh_count -= 1
                if 0 <= x < rows and 0 <= y + 1 < cols and grid[x][y + 1] == 1:
                    grid[x][y + 1] = 2
                    queue.append((x, y + 1))
                    fresh_count -= 1
                if 0 <= x< rows and 0 <= y - 1 < cols and grid[x][y - 1] == 1:
                    grid[x][y - 1] = 2
                    queue.append((x, y - 1))
                    fresh_count -= 1
            if fresh_count == 0:
                return mins
        
        return -1
```

## [207. Course Schedule](https://leetcode.com/problems/course-schedule/)

### **Description**

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `numCourses = 2, prerequisites = [[1,0]]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `numCourses = 2, prerequisites = [[1,0],[0,1]]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 这个问题可以转化为判断 **有向图是否存在环** 的问题。首先构建图，key 为课程，value 为先修课程。然后使用深度优先搜索（DFS），遍历图中的每个节点，并创建其克隆。使用 `visited` 数组来存储已访问的节点，以避免重复访问，`0` 表示未访问，`1` 表示正在访问，`2` 表示已访问。假如在遍历过程中，发现某个节点正在访问，则说明存在环，返回 `false`，否则返回 `true`。

**Complexity:** Time: _O(V + E)_, Space: _O(V)_

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)
        for course, pre in prerequisites:
            graph[pre].append(course)
        
        visited = [0] * numCourses

        def dfs(node):
            if visited[node] == 1:
                return False
            if visited[node] == 2:
                return True

            visited[node] = 1
            for neighbor in graph[node]:
                if not dfs(neighbor):
                    return dfs(neighbor)
            visited[node] = 2
            return True

        for course in range(numCourses):
            if not dfs(course):
                return False
        
        return True
```

## [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

### **Description**

A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `Trie trie = new Trie(); trie.insert("apple"); trie.search("apple"); // returns true trie.search("app"); // returns false trie.startsWith("app"); // returns true trie.insert("app"); trie.search("app"); // returns true`
- **Output**: `[null, true, false, true, null, true]`

---

**Example 2:**

- **Input**: `Trie trie = new Trie(); trie.insert("apple"); trie.search("apple"); // returns true trie.search("app"); // returns false trie.startsWith("app"); // returns true trie.insert("app"); trie.search("app"); // returns true`
- **Output**: `[null, true, false, true, null, true]`

---

</details>

### **Solution**

**Idea:** Trie（前缀树）是一种字典树，适用于高效地存储和查询字符串前缀。核心思想是：每个节点代表一个字符，子节点表示当前路径的下一个字符。

每个节点包含：
- `children`（存储子节点的字典）
- `is_end`（标记当前路径是否构成完整单词）

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Trie:

    def __init__(self):
        self.children = {}  # 存储子节点
        self.is_end = False # 记录是否是完整单词的结尾

    def insert(self, word: str) -> None:
        node = self  # 从根节点开始
        for ch in word:
            if ch not in node.children:
                node.children[ch] = Trie()  # 创建新节点
            node = node.children[ch]  # 移动到下一个字符
        node.is_end = True  # 标记单词结尾

    def search(self, word: str) -> bool:
        node = self._findNode(word)
        return node is not None and node.is_end

    def startsWith(self, prefix: str) -> bool:
        return self._findNode(prefix) is not None
        
    def _findNode(self, prefix: str):
        """辅助函数，找到 prefix 对应的最后一个节点"""
        node = self
        for ch in prefix:
            if ch not in node.children:
                return None  # 不存在该前缀
            node = node.children[ch]
        return node  # 返回匹配到的最后一个节点
```

---

`Top Interview 150 补充`

---

## [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

### **Description**

Given an `m x n` matrix `board` containing `'X'` and `'O'`, capture all regions that are 4-directionally surrounded by `'X'`.

A region is captured by flipping all `'O'`s into `'X'`s in that surrounded region.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`
- **Output**: `[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]`

---

**Example 2:**

- **Input**: `board = [["X"]]`
- **Output**: `[["X"]]`

---

</details>

### **Solution**

**Idea:** 使用深度优先搜索（DFS），将所有与边界相连的 `'O'` 标记为 `'Y'`，然后遍历整个矩阵，将未被标记的 `'O'` 翻转为 `'X'`，将标记的 `'O'` 翻转回 `'O'`。

**Complexity:** Time: _O(m * n)_, Space: _O(m * n)_

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        rows, cols = len(board), len(board[0])

        def dfs(row, col):
            if row < 0 or row >= rows or col < 0 or col >= cols or board[row][col] != 'O':
                return 
            board[row][col] = 'Y'
            dfs(row - 1, col)
            dfs(row + 1, col)
            dfs(row, col - 1)
            dfs(row, col + 1)

        # 处理边界上的 'O'
        for row in range(rows):
            for col in [0, cols - 1]:
                if board[row][col] == 'O':
                    dfs(row, col)
        for col in range(cols):
            for row in [0, rows - 1]:
                if board[row][col] == 'O':
                    dfs(row, col)

        # 遍历整个矩阵，将未被标记的 'O' 翻转为 'X'，将标记的 'O' 翻转回 'O'
        for row in range(rows):
            for col in range(cols):
                    if board[row][col] == 'O':
                        board[row][col] = 'X'
                    if board[row][col] == 'Y':
                        board[row][col] = 'O'

        return board    
```

## [133. Clone Graph](https://leetcode.com/problems/clone-graph/)

### **Description**

Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list of its neighbors.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `adjList = [[2,4],[1,3],[2,4],[1,3]]`
- **Output**: `[[2,4],[1,3],[2,4],[1,3]]`

---

</details>

### **Solution**

**Idea:** 使用深度优先搜索（DFS），遍历图中的每个节点，并创建其克隆。使用 `visited` 字典来存储已访问的节点，以避免重复访问，key 为原节点，value 为克隆节点。

**Complexity:** Time: _O(V + E)_, Space: _O(V)_

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        if not node:
            return None
        
        visited = {}

        def dfs(node: Optional['Node']):
            if node in visited:
                return visited[node]
            
            clone = Node(node.val, [])
            visited[node] = clone

            for n in node.neighbors:
                clone.neighbors.append(dfs(n))
            
            return clone
        
        return dfs(node)
```

## [399. Evaluate Division](https://leetcode.com/problems/evaluate-division/)

### **Description**

You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.

You are also given some queries, where `queries[j] = [Cj, Dj]` represents the `j-th` query where you must find the answer for `Cj / Dj = ?`.

Return the answers to all queries. If a single answer cannot be determined, return `-1.0`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]`
- **Output**: `[6.00000,0.50000,-1.00000,1.00000,-1.00000]`

---

**Example 2:**

- **Input**: `equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]`
- **Output**: `[3.75000,0.40000,5.00000,0.20000]`

---

</details>

### **Solution**

**Idea:** 首先构建图，将每个变量视为节点，将每个等式视为有向边，边上的权重为等式中的值。然后使用深度优先搜索（DFS），从起点开始，沿着边遍历图，计算结果。

**Complexity:** Time: _O(V + E)_, Space: _O(V)_

```python
class Solution:
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        graph = defaultdict(dict)
        for (x, y), value in zip(equations, values):
            graph[x][y] = value
            graph[y][x] = 1 / value

        def dfs(start, end):
            if start == end:
                return 1.0
            visited.append(start)
            for neighbor, value in graph[start].items():
                if neighbor not in visited:
                    result = dfs(neighbor, end)
                    if result != -1.0:
                        return value * result
            return -1.0
        
        result = []
        for start, end in queries:
            if start not in graph or end not in graph:
                result.append(-1.0)
            else:
                visited = []
                result.append(dfs(start, end))
        
        return result
```

## [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)

### **Description**

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you must take course `bi` first if you want to take course `ai`.

For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `numCourses = 2, prerequisites = [[1,0]]`
- **Output**: `[0,1]`

---

**Example 2:**

- **Input**: `numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]`
- **Output**: `[0,2,1,3]`

---

</details>

### **Solution**

**Idea:** 这题和上一题基本一致，只不过需要返回课程的顺序。思路还是使用 DFS，在遍历过程中，如果发现某个节点正在访问，则说明存在环，返回 `false`，否则返回 `true`。需要注意 `order` 的顺序，最后需要逆序输出。

**Complexity:** Time: _O(V + E)_, Space: _O(V)_

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = defaultdict(list)
        for course, pre in prerequisites:
            graph[pre].append(course)

        visited = [0] * numCourses
        order = []

        def dfs(node):
            if visited[node] == 1:
                return False
            if visited[node] == 2:
                return True

            visited[node] = 1
            for neighbor in graph[node]:
                if not dfs(neighbor): 
                    return False
            visited[node] = 2
            order.append(node)
            return True

        for course in range(numCourses):
            if not dfs(course):
                return []
                    
        return order[::-1]
```