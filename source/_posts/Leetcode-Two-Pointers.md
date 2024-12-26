---
title: Leetcode Two Pointers
date: 2024-12-25 23:07:23
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)

### **Description**

A phrase is a **palindrome** if, after converting all uppercase letters to lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a **palindrome**, or `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "A man, a plan, a canal: Panama"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `s = "race a car"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用双指针，一个从左到右，一个从右到左，比较两个指针指向的字符是否相同。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if not s:
            return True

        i = 0
        j = len(s) - 1

        while i < j:
            while i < j and not s[i].isalnum():
                i += 1
            while i < j and not s[j].isalnum():
                j -= 1
            if s[i].lower() != s[j].lower():
                return False
            i += 1
            j -= 1

        return True
```

## [392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

### **Description**

Given two strings `s` and `t`, return `true` if `s` is a **subsequence** of `t`, or `false` otherwise.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (possibly zero) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "abc", t = "ahbgdc"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `s = "axc", t = "ahbgdc"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用双指针，一个从左到右遍历 `s`，一个从左到右遍历 `t`，如果 `s[i] == t[j]`，则 `i` 向右移动，`j` 向右移动。如果 `i` 遍历完 `s`，则返回 `true`，否则返回 `false`。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def isSubsequence(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        i = 0
        j = 0
        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
            j += 1
        return i == len(s)
```

## [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### **Description**

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `numbers = [2, 7, 11, 15], target = 9`
- **Output**: `[1, 2]`

---

**Example 2:**

- **Input**: `numbers = [2, 3, 4], target = 6`
- **Output**: `[1, 3]`

---

</details>

### **Solution**

**Idea:** 使用双指针，一个从左到右，一个从右到左，如果 `numbers[i] + numbers[j] == target`，则返回 `[i + 1, j + 1]`，如果 `numbers[i] + numbers[j] > target`，则 `j` 向左移动，如果 `numbers[i] + numbers[j] < target`，则 `i` 向右移动（因为数组是**不减**的）。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        i = 0
        j = len(numbers) - 1

        while i < j :
            if numbers[i] + numbers[j] == target:
                return [i + 1, j + 1]
            elif numbers[i] + numbers[j] > target:
                j -= 1
            else:
                i += 1
```

## [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

### **Description**

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `i`-th line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]`
- **Output**: `49`

![Container With Most Water](/img/leetcode11.png)

---

**Example 2:**

- **Input**: `height = [1, 1]`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用双指针，一个从左到右，一个从右到左。比较 `height[i]` 和 `height[j]`，其中小的那个向中间移动（因为面积取决于短的那一边），并计算面积。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        i = 0
        j = 0
        ans = 0
        sub = list()

        while j < len(s):
            while s[j] in sub:
                sub.pop(0)
                i += 1
            sub.append(s[j])
            ans = max(ans, j - i + 1)
            j += 1

        return ans
```

## [15. 3Sum](https://leetcode.com/problems/3sum/)

### **Description**

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [-1, 0, 1, 2, -1, -4]`
- **Output**: `[[-1, -1, 2], [-1, 0, 1]]`

---

**Example 2:**

- **Input**: `nums = [0, 1, 1]`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 这道题最直观的方法是三重循环，时间复杂度是 _O(n^3)_ 。那么如何优化呢？

可以发现，如果我们固定了前两重循环枚举到的元素 `a` 和 `b`，那么只有唯一的 `c` 满足 `a + b + c = 0`。当 **第二重循环** 往后枚举一个元素 `b'` 时，由于 `b' > b`，那么满足 `a + b' + c' = 0` 的 `c'` 一定有 `c' < c`，即 `c'` 在数组中一定出现在 `c` 的左侧。也就是说，我们可以从小到大枚举 `b`，同时从大到小枚举 `c`，即 **第二重循环** 和 **第三重循环** 实际上是 **并列** 的关系。这样，我们就可以在 **第二重循环** 中使用 **双指针**，从而将时间复杂度优化到 _O(n^2)_。

**Complexity:** Time: _O(n^2)_, Space: _O(log n)_

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        ans = list()
        nums.sort()

        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            target = -nums[i]
            k = len(nums) - 1
            for j in range(i + 1, len(nums)):
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue
                while j < k and nums[j] + nums[k] > target:
                    k -= 1
                if j == k:
                    break
                if nums[j] + nums[k] == target:
                    ans.append([nums[i], nums[j], nums[k]])

        return ans
```
