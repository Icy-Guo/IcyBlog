---
title: Leetcode Sliding Window
date: 2024-12-15 10:52:35
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### **Description**

Given a string `s`, find the length of the longest substring without repeating characters.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "abcabcbb"`
- **Output**: `3`

---

**Example 2:**

- **Input**: `s = "bbbbb"`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用滑动窗口。当窗口内的字符串不包含重复字符时，记录长度，并尝试缩小窗口，即 `i` 向右移动。缩小到不满足条件时，再尝试扩大窗口，即 `j` 向右移动。

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

## [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

### **Description**

Given two strings `s` and `p`, return an array of all the start indices of `p`'s anagrams in `s`. You may return the answer in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "cbaebabacd", p = "abc"`
- **Output**: `[0,6]`

---

**Example 2:**

- **Input**: `s = "abab", p = "ab"`
- **Output**: `[0,1,2]`

---

</details>

### **Solution**

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        cnt_p = {}
        cnt_s = {}
        for char in p:
            cnt_p[char] = cnt_p.get(char, 0) + 1

        ans = []

        for i in range(len(s)):
            cnt_s[s[i]] = cnt_s.get(s[i], 0) + 1

            if i >= len(p):
                left_char = s[i - len(p)]

                if cnt_s[left_char] == 1:
                    del cnt_s[left_char]
                else:
                    cnt_s[left_char] -= 1

            if cnt_s == cnt_p:
                ans.append(i - len(p) + 1)
        
        return ans
```

---

`Top Interview 150 补充`

---

## [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

### **Description**

Given an array of positive integers `nums` and a positive integer `target`, return the minimal length of a subarray of `nums` such that the sum of the subarray is at least `target`. If there isn't one, return `0` instead.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [2, 3, 1, 2, 4, 3], target = 7`
- **Output**: `2`

---

**Example 2:**

- **Input**: `nums = [1, 4, 4], target = 4`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用滑动窗口。当窗口内的和大于等于 `target` 时，记录长度，并尝试缩小窗口，即 `i` 向右移动。缩小到不满足条件时，再尝试扩大窗口，即 `j` 向右移动。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        """
        :type target: int
        :type nums: List[int]
        :rtype: int
        """
        i = 0
        j = 0
        ans = len(nums) + 1
        total = 0

        while j < len(nums):
            total += nums[j]
            while total >= target:
                total -= nums[i]
                ans = min(ans, j - i + 1)
                i += 1
            j += 1

        return 0 if ans == len(nums) + 1 else ans
```
