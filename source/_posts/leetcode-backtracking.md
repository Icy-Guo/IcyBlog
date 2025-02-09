---
title: Leetcode Backtracking
date: 2025-01-24 09:25:15
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [46. Permutations](https://leetcode.com/problems/permutations/)

### **Description**

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,2,3]`
- **Output**: `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

---

**Example 2:**

- **Input**: `nums = [0,1]`
- **Output**: `[[0,1],[1,0]]`

---

</details>

### **Solution**

**Idea:** 使用回溯法来生成所有可能的排列。通过递归地选择每个元素，并确保每个元素只被选择一次，来构建所有可能的排列。注意复制路径时需要使用 `path[:]` 来创建一个新的列表，而不是直接使用 `path`，因为 `path` 是一个引用，直接赋值会导致结果不正确。

**Complexity:** Time: _O(n * n!)_, Space: _O(n)_ 时间复杂度为 _O(n * n!)_ 的原因是 `backtrack` 函数调用次数为 _O(n!)_，将答案添加到 `ans` 中需要 _O(n)_ 的时间。

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtrack(path, used):
            if len(path) == len(nums):
                ans.append(path[:])
                return

            for num in nums:
                if num in used:
                    continue
                used.add(num)
                path.append(num)

                backtrack(path, used)

                used.remove(num)
                path.pop()

        backtrack([], set())
        return ans
```

## [78. Subsets](https://leetcode.com/problems/subsets/)

### **Description**

Given an integer array `nums` of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,2,3]`
- **Output**: `[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

---

**Example 2:**

- **Input**: `nums = [0]`
- **Output**: `[[],[0]]`

---

</details>

### **Solution**

**Idea:** 使用回溯法来生成所有可能的子集。通过递归地选择每个元素，并确保每个元素只被选择一次，来构建所有可能的子集。注意复制路径时需要使用 `path[:]` 来创建一个新的列表，而不是直接使用 `path`，因为 `path` 是一个引用，直接赋值会导致结果不正确。

**Complexity:** Time: _O(n * 2^n)_, Space: _O(n)_

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = []

        def backtrack(start, path):
            ans.append(path[:])
            for i in range(start, len(nums)):
                path.append(nums[i])
                backtrack(i + 1, path)
                path.pop()
        
        backtrack(0, [])
        return ans
```

## [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

### **Description**

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `digits = "23"`
- **Output**: `["ad","ae","af","bd","be","bf","cd","ce","cf"]`

---

**Example 2:**

- **Input**: `digits = ""`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 使用回溯法来生成所有可能的组合。通过递归地选择每个数字，并确保每个数字只被选择一次，来构建所有可能的组合。

**Complexity:** Time: _O(n * 4^n)_, Space: _O(n)_ 每个数字最多有 4 个字母，所以时间复杂度为 _O(4^n)_，将答案添加到 `ans` 中需要 _O(n)_ 的时间。

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        phone_map = {
            "2": "abc", "3": "def", "4": "ghi", "5": "jkl",
            "6": "mno", "7": "pqrs", "8": "tuv", "9": "wxyz"
        }

        ans = []

        if not digits:
            return []

        def backtrack(index, path):
            if index == len(digits):
                ans.append(''.join(path))
                return
            
            letters = phone_map[digits[index]]

            for letter in letters:
                path.append(letter)
                backtrack(index + 1, path)
                path.pop()
        
        backtrack(0, [])
        return ans
```

## [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

### **Description**

Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `candidates = [2,3,6,7], target = 7`
- **Output**: `[[2,2,3],[7]]`

---

**Example 2:**

- **Input**: `candidates = [2,3,5], target = 8`
- **Output**: `[[2,2,2,2],[2,3,3],[3,5]]`

---

</details>

### **Solution**

**Idea:** 使用回溯法来生成所有可能的组合。通过递归地选择每个数字，并确保每个数字只被选择一次，来构建所有可能的组合。

**Complexity:** Time: _O(n * 2^n)_, Space: _O(n)_ 

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        ans = []

        def backtrack(remaining, start, path):
            if remaining == 0:
                ans.append(path[:])
            
            for i in range(start, len(candidates)):
                if candidates[i] > remaining:
                    continue
                path.append(candidates[i])
                backtrack(remaining - candidates[i], i, path)
                path.pop() 
        
        backtrack(target, 0, [])
        return ans
```

## [22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

### **Description**

Given an integer `n`, return all combinations of well-formed parentheses.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `n = 3`
- **Output**: `["((()))","(()())","(())()","()(())","()()()"]`

---

**Example 2:**

- **Input**: `n = 1`
- **Output**: `["()"]`

---

</details>

### **Solution**

**Idea:** 使用回溯法来生成所有可能的括号组合。通过递归地选择每个括号，并确保每个括号只被选择一次，来构建所有可能的括号组合。

**Complexity:** Time: _O(4^n / √n)_, Space: _O(n)_ 这是第 n 个卡特兰数的渐近表达式，表示生成所有有效括号组合的时间复杂度。

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []

        def backtrack(str, left_count, right_count):
            if len(str) == 2 * n:
                ans.append(str)
                return
            
            if left_count < n:
                backtrack(str + '(', left_count + 1, right_count)
            
            if right_count < left_count:
                backtrack(str + ')', left_count, right_count + 1)

        backtrack('', 0, 0)
        return ans
```

## [79. Word Search](https://leetcode.com/problems/word-search/)

### **Description**

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"`
- **Output**: `true`

---

</details>

### **Solution**

```python

```
