---
title: Leetcode Array & String Part 1
date: 2024-12-11 22:45:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## 前言

准备技术面试时，算法题是必不可少的一部分。本系列博客将记录本人在 Leetcode 刷题过程中的解题思路和心得体会，目前的目标是完成 `Top Interview 150` 题目。这些题目精选自常见的技术面试场景，涵盖了数组、链表、树、图、动态规划等多个重要领域，是巩固基础和提升实战能力的绝佳选择。通过这一系列的文章，希望不仅能够帮助自己梳理和总结解题思路，也能为同样在刷题路上的朋友提供一些启发和参考。🖥️✨

## [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

### **Description**

You are given two sorted integer arrays `nums1` and `nums2`, and two integers `m` and `n` representing the number of elements in each array. Your task is to merge these arrays into a single sorted array.

<details>
<summary><b>Click to view full description</b></summary>

The merged result should be stored in `nums1`, which has enough space to hold all elements. The initial extra space in `nums1` is represented by `0`s and should be ignored during the merge.

---

**Example 1:**

- **Input**: `nums1 = [1, 2, 3, 0, 0, 0], m = 3`, `nums2 = [2, 5, 6], n = 3`
- **Output**: `[1, 2, 2, 3, 5, 6]`

---

**Example 2:**

- **Input**: `nums1 = [1], m = 1`, `nums2 = [], n = 0`
- **Output**: `[1]`

</details>

### **Solution**

**Idea:** 双指针是数组问题中常用的技巧。本题中，我们使用两个指针分别指向 `nums1` 和 `nums2` 的末尾，然后从后往前遍历，将较大的元素插入到 `nums1` 的末尾。本题的难点在于需要从后往前遍历，因为 `nums1` 的末尾是 `0`，我们需要将较大的元素插入到 `nums1` 的末尾，这样就不会覆盖 `nums1` 原有的元素。

**Complexity:** Time: _O(m + n)_, Space: _O(1)_

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        p1 = m - 1
        p2 = n - 1
        p = m + n -1

        while p2 >= 0:
            if p1 >= 0 and nums1[p1] >= nums2[p2]:
                nums1[p] = nums1[p1]
                p1 -= 1
            else:
                nums1[p] = nums2[p2]
                p2 -= 1
            p -= 1
```

## [27. Remove Element](https://leetcode.com/problems/remove-element/)

### **Description**

Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` in-place. The order of the elements may be changed. Then return the number of elements in `nums` which are not equal to `val`.

<details>
<summary><b>Click to view full description</b></summary>

Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.

Return `k`.

---

**Example 1:**

- **Input**: `nums = [3, 2, 2, 3]`, `val = 3`
- **Output**: `2`, `nums = [2, 2, _, _]`

---

**Example 2:**

- **Input**: `nums = [0, 1, 2, 2, 3, 0, 4, 2]`, `val = 2`
- **Output**: `5`, `nums = [0, 1, 4, 0, 3, _, _, _]`

</details>

### **Solution**

**Idea:** 经典双指针问题。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        i = 0
        j = 0

        while i <= len(nums) - 1:
            if nums[i] != val:
                nums[j] = nums[i]
                j += 1
            i += 1

        return j
```

## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

### **Description**

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates **in-place** such that each unique element appears only **once**. The relative order of the elements should be kept the same. Then return the number of unique elements in `nums`.

<details>
<summary><b>Click to view full description</b></summary>

Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.

Return `k`.

---

**Example 1:**

- **Input**: `nums = [1, 1, 2]`
- **Output**: `2`, `nums = [1, 2, _]`

---

**Example 2:**

- **Input**: `nums = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]`
- **Output**: `5`, `nums = [0, 1, 2, 3, 4, _, _, _, _, _]`

</details>

### **Solution**

**Idea:** 经典双指针问题。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i = 1
        j = 1

        while i <= len(nums) - 1:
            if nums[i] != nums [i - 1]:
                nums[j] = nums[i]
                j += 1
            i += 1

        return j
```

## [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

### **Description**

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

<details>
<summary><b>Click to view full description</b></summary>

Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.

Return `k`.

---

**Example 1:**

- **Input**: `nums = [1, 1, 1, 2, 2, 3]`
- **Output**: `5`, `nums = [1, 1, 2, 2, 3, _]`

---

**Example 2:**

- **Input**: `nums = [0, 0, 1, 1, 1, 1, 2, 3, 3]`
- **Output**: `7`, `nums = [0, 0, 1, 1, 2, 3, 3, _, _]`

</details>

### **Solution**

**Idea:** 经典双指针问题，区别在于判断条件，要想明白快指针和谁比较。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i = 2
        j = 2

        while i <= len(nums) - 1:
            if nums[i] != nums[j - 2]:
                nums[j] = nums[i]
                j += 1
            i += 1

        return j
```

## [169. Majority Element](https://leetcode.com/problems/majority-element/)

### **Description**

Given an array `nums` of size `n`, return the majority element.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [3, 2, 3]`
- **Output**: `3`

---

**Example 2:**

- **Input**: `nums = [2, 2, 1, 1, 1, 2, 2]`
- **Output**: `2`

</details>

### **Solution**

#### **Approach 1** 哈希表

**Idea:** 这道题直觉上很容易想到哈希表，用哈希表统计每个元素出现的次数，然后遍历哈希表，找到出现次数大于 `n // 2` 的元素。创建字典时 `defaultdict` 的用法值得积累。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution:
    def majorityElement(self, nums) -> int:
        n = len(nums)
        m = defaultdict(int) # 对于不存在的 key，自动初始化为 0

        for num in nums:
            m[num] += 1

        threshold = n // 2
        for key, value in m.items():
            if value > threshold:
                return key

        return 0
```

#### **Approach 2** 摩尔投票法

**Idea:** 摩尔投票法是一种高效的算法，用于解决寻找多数元素的问题，是本题的最优解。它的核心思想是通过不断消除不相同的元素，最终剩下的元素就是多数元素。当 `count` 为 0 时，`candidate` 更新为当前元素。然后继续遍历，如果当前元素和 `candidate` 相同，`count` 加 1，否则 `count` 减 1。最后剩下的 `candidate` 就是多数元素。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        candidate = 0
        count = 0

        for num in nums:
            if count == 0:
                candidate = num
            if num == candidate:
                count += 1
            else:
                count -= 1

        return candidate
```
