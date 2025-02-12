---
title: Leetcode Binary Search
date: 2025-02-12 09:46:25
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)

### **Description**

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,3,5,6], target = 5`
- **Output**: `2`

---

**Example 2:**

- **Input**: `nums = [1,3,5,6], target = 2`
- **Output**: `1`

</details>

### **Solution**

**Idea:** 使用二分查找，找到目标值，如果目标值不存在，则返回插入位置。

**Complexity:** Time: _O(log n)_, Space: _O(1)_

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            if nums[mid] < target:
                left = mid + 1
            if nums[mid] > target:
                right = mid - 1
                
        return left
```

## [74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

### **Description**

Write an efficient algorithm that searches for a value target in an m x n integer matrix. This matrix has the following properties:

- Each row is sorted in ascending order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in matrix or `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3`
- **Output**: `true`

---

**Example 2:**

- **Input**: `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13`
- **Output**: `false`

</details>

### **Solution**

**Idea:** 将二维矩阵看作一维有序数组，使用二分查找，找到目标值。如果目标值不存在，则返回 `false`。

**Complexity:** Time: _O(log m * n)_, Space: _O(1)_

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or not matrix[0]:
            return False

        m = len(matrix)
        n = len(matrix[0])
        left = 0
        right = m * n - 1

        while left <= right:
            mid = (left + right) // 2
            row = mid // n
            col = mid % n
            if matrix[row][col] == target:
                return True
            if matrix[row][col] < target:
                left = mid + 1
            if matrix[row][col] > target:
                right = mid - 1
        
        return False
```

## [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

### **Description**

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending positions of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [5,7,7,8,8,10], target = 8`
- **Output**: `[3,4]`

---

**Example 2:**

- **Input**: `nums = [5,7,7,8,8,10], target = 6`
- **Output**: `[-1,-1]`

</details>

### **Solution**

**Idea:** 使用两次二分查找，分别找到目标值的左边界和右边界。

**Complexity:** Time: _O(log n)_, Space: _O(1)_

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def findLeft(nums, target):
            left, right = 0, len(nums) - 1
            while left <= right:
                mid = (left + right) // 2
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid - 1
            return left
        
        def findRight(nums, target):
            left, right = 0, len(nums) - 1
            while left <= right:
                mid = (left + right) // 2
                if nums[mid] <= target:
                    left = mid + 1
                else:
                    right = mid - 1
            return right

        left_index = findLeft(nums, target)
        right_index = findRight(nums, target)

        if left_index <= right_index:
            return [left_index, right_index]
        else:
            return [-1, -1]
```

## [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

### **Description**

There is an integer array `nums` sorted in ascending order (with distinct values).

Given the array `nums` after the possible rotation and an integer `target`, return the index of `target` if it is in `nums`, or `-1` if it is not in `nums`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [4,5,6,7,0,1,2], target = 0`
- **Output**: `4`

---

**Example 2:**

- **Input**: `nums = [4,5,6,7,0,1,2], target = 3`
- **Output**: `-1`

</details>

### **Solution**

**Idea:** 使用二分查找，找到目标值。

1. 找到有序的那一部分：
- 旋转数组总有一部分是有序的
- 如果 `nums[left] <= nums[mid]`，则 `left` 到 `mid` 是有序的
- 否则，`mid` 到 `right` 是有序的
  
2. 判断 `target` 是否在有序的那一部分：
- 如果 `target` 在有序的那一部分，则继续二分查找
- 否则，`target` 在另一部分，继续二分查找

**Complexity:** Time: _O(log n)_, Space: _O(1)_

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid
            
            # 判断左边是否有序
            if nums[left] <= nums[mid]:
                # 判断target是否在左边
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            
            # 判断右边是否有序
            else:
                # 判断target是否在右边
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        
        return -1
```

## [153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

### **Description**

Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

- `[4,5,6,7,0,1,2]` if it was rotated `4` times.
- `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of unique elements, return the minimum element of this array.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [3,4,5,1,2]`
- **Output**: `1`

---

**Example 2:**

- **Input**: `nums = [4,5,6,7,0,1,2]`
- **Output**: `0`

---

</details>

### **Solution**

**Idea:** 旋转后的数组可以分为两部分： **左侧部分（较大值）** 和 **右侧部分（较小值）**。

- 如果 `nums[mid] > nums[right]`，则最小值在右侧部分，继续二分查找
- 否则，最小值在左侧部分或者就是 `nums[mid]`，继续二分查找

**Complexity:** Time: _O(log n)_, Space: _O(1)_

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left = 0
        right = len(nums) - 1

        while left < right:
            mid = (left + right) // 2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid

        return nums[left]
```
