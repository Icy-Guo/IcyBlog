---
title: Leetcode Intervals
date: 2025-01-06 23:20:21
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [228. Summary Ranges](https://leetcode.com/problems/summary-ranges/)

### **Description**

You are given a sorted unique integer array `nums`.

A range `[a, b]` is the set of all integers from `a` to `b` (inclusive).

Return the smallest sorted list of ranges that cover all the numbers in the array exactly. That is, each element of `nums` is covered by exactly one of the ranges, and there is no integer `x` such that `x` is in one of the ranges but not in `nums`.

Each range `[a, b]` in the list should be output as:

- `"a->b"` if `a != b`
- `"a"` if `a == b`

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [0, 1, 2, 4, 5, 7]`
- **Output**: `["0->2", "4->5", "7"]`

---

**Example 2:**

- **Input**: `nums = [0, 2, 3, 4, 6, 8, 9]`
- **Output**: `["0", "2->4", "6", "8->9"]`

---

</details>

### **Solution**

**Idea:** 遍历 `nums`，如果当前元素与前一个元素连续，则继续遍历。否则，根据范围的起点和终点（包含单个值或多个值），将范围加入 `ans`。最后需要处理最后一个范围。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        ans = []
        low = -1

        if not nums:
            return ans

        for i, num in enumerate(nums):
            if low == -1: # 初始化
                low = i
                continue
            if num == nums[i - 1] + 1:
                continue
            else:
                if low == i - 1:
                    ans.append(f"{nums[low]}")
                else:
                    ans.append(f"{nums[low]}->{nums[i - 1]}")
                low = i
        if low == len(nums) - 1:
            ans.append(f"{nums[low]}")
        else:
            ans.append(f"{nums[low]}->{nums[-1]}")

        return ans
```

## [56. Merge Intervals](https://leetcode.com/problems/merge-intervals/)

### **Description**

Given an array of intervals where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `intervals = [[1, 3], [2, 6], [8, 10], [15, 18]]`
- **Output**: `[[1, 6], [8, 10], [15, 18]]`

---

**Example 2:**

- **Input**: `intervals = [[1, 4], [4, 5]]`
- **Output**: `[[1, 5]]`

---

</details>

### **Solution**

**Idea:** 先对 `intervals` 进行排序，然后遍历 `intervals`，如果当前区间的 `start` 小于等于结果集中最后一个区间的 `end`，则将当前区间与结果集中最后一个区间合并，并更新结果集中最后一个区间的 `end`。否则，将当前区间加入结果集。

**Complexity:** Time: _O(n log n)_，Space: _O(log n)_

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        ans = []
        intervals.sort(key = lambda x: x[0])

        for interval in intervals:
            if not ans or ans[-1][1] < interval[0]:
                ans.append(interval)
            else:
                ans[-1][1] = max(ans[-1][1], interval[1])
        return ans
```

## [57. Insert Interval](https://leetcode.com/problems/insert-interval/)

### **Description**

You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [start_i, end_i]` represent the start and the end of the `i-th` interval and `intervals` is sorted in ascending order by `start_i`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `start_i` and `intervals` does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `intervals = [[1, 3], [6, 9]], newInterval = [2, 5]`
- **Output**: `[[1, 5], [6, 9]]`

---

**Example 2:**

- **Input**: `intervals = [[1, 2], [3, 5], [6, 7], [8, 10], [12, 16]], newInterval = [4, 8]`
- **Output**: `[[1, 2], [3, 10], [12, 16]]`

---

</details>

### **Solution**

**Idea:** 遍历 `intervals`，如果当前区间的 `end` 小于 `newInterval` 的 `start`，则将当前区间加入 `ans`，并继续遍历。如果当前区间的 `start` 大于 `newInterval` 的 `end`，则将 `newInterval` 加入 `ans`，并将之后的区间直接加入 `ans`。否则，将 `newInterval` 与当前区间合并，并更新 `newInterval`。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        ans = []
        for interval in intervals:
            if newInterval[1] < interval[0]:
                ans.append(newInterval)
                ans.extend(intervals[intervals.index(interval):])
                return ans
            elif newInterval[0] > interval[1]:
                ans.append(interval)
            else:
                newInterval = [min(newInterval[0], interval[0]), max(newInterval[1], interval[1])]

        ans.append(newInterval)
        return ans
```

## [452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)

### **Description**

There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [x_start, x_end]` denotes a balloon whose horizontal diameter stretches between `x_start` and `x_end`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with `x_start` and `x_end` is burst by an arrow shot at `x` if `x_start <= x <= x_end`. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return the minimum number of arrows that must be shot to burst all balloons.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `points = [[1, 2], [3, 4], [5, 6], [7, 8]]`
- **Output**: `4`

---

**Example 2:**

- **Input**: `points = [[1, 2], [2, 3], [3, 4], [4, 5]]`
- **Output**: `2`

---

</details>

### **Solution**

**Idea:** 先对 `points` 进行排序。然后使用贪心算法的思想，首先将第一个区间的 `x_end` 作为 `shot`，然后遍历 `points`。如果当前区间的 `x_start` 大于 `shot`，则说明之前的箭无法射中当前区间，需要射箭，射箭次数加一，并更新 `shot` 为当前区间的 `x_end`。

**Complexity:** Time: _O(n log n)_，Space: _O(log n)_

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if not points:
            return 0

        points.sort(key = lambda p: p[1])
        shot = points[0][1]
        ans = 1
        for p in points:
            if p[0] > shot:
                ans += 1
                shot = p[1]
        return ans
```
