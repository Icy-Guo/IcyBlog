---
title: Leetcode Sliding Window
date: 2024-12-26 10:14:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
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

## 近期题目总结

最近刷完了 `Top Interview 150` 中的 `Array & String`，`Two Pointers` 和 `Sliding Window` 的题目。是时候做个总结了 🧑🏻‍💻。这部分题目主要考察的是**数组**和**字符串**的操作，双指针和滑动窗口也是数组和字符串相关题目的常用技巧。下面就总结一下这部分题目常见的解题思路和技巧。

### 1. 双指针

双指针是处理数组和字符串问题时最常用的技巧之一。定义两个指针变量，它们在一个数组（或字符串）上独立移动，彼此之间可以有各种移动逻辑关系。

#### 1.1 对撞指针

- 两个指针分别从数组的头部和尾部向中间移动
- 适用场景：查找、比较等，通常用于有序数组
- 典型题目：
  - [125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
  - [167. Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

#### 1.2 快慢指针

- 两个指针从同一位置出发，但移动速度不同
- 适用场景：原地修改数组、去重等
- 典型题目：
  - [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
  - [27. Remove Element](https://leetcode.com/problems/remove-element/)

### 2. 滑动窗口

- 滑动窗口是一种特殊的双指针技巧，通常用于在一个区间范围内动态维护某种信息（如和、最小值、最大值）
- 适用场景：查找满足条件的最短或最长子数组；子字符串问题（如无重复字符的最长子串）；和相关的问题（如求和等于某值的连续子数组）
- 典型题目：
  - [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
  - [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

### 3. 哈希表

- 哈希表是一种用于快速查找和统计元素出现次数的数据结构，插入、删除、查找的时间复杂度为 _O(1)_
- 适用场景：查找、计数、去重等
- 典型题目：
  - [169. Majority Element](https://leetcode.com/problems/majority-element/)
  - [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

### 4. 排序

- 排序是指先对数组进行排序，然后根据排序后的结果进行查找、比较等操作，减少时间复杂度。排序的时间复杂度为 _O(n log n)_ ，空间复杂度为 _O(log n)_
- 适用场景：涉及顺序或相对大小关系
- 典型题目：
  - [274. H-Index](https://leetcode.com/problems/h-index/)

### 5. 二分查找

- 二分查找是一种高效的查找算法，适用于有序数组，时间复杂度为 _O(log n)_
- 适用场景：数组有序或单调性问题
- 典型题目：
  - [274. H-Index](https://leetcode.com/problems/h-index/)

### 6. 贪心算法

- 贪心算法指每一步都做出局部最优解，从而达到全局最优解
- 适用场景：局部最优解可以推导出全局最优解
- 典型题目：
  - [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
  - [55. Jump Game](https://leetcode.com/problems/jump-game/)

### 7. 动态规划

- 动态规划将问题分解为子问题，并存储子问题的解
- 适用场景：最优化问题
- 典型题目：
  - [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
