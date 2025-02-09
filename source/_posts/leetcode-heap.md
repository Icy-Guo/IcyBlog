---
title: Leetcode Heap
date: 2025-02-08 09:47:19
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### **Description**

Given an integer array `nums`, return the `kth` largest element in the array.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [3,2,1,5,6,4]`, `k = 2`
- **Output**: `5`

---

**Example 2:**

- **Input**: `nums = [3,2,3,1,2,4,5,5,6]`, `k = 4`
- **Output**: `4`

---

</details>

### **Solution**

**Idea:** 使用小顶堆来存储数组，堆的大小为 `k`，堆顶元素即为第 `k` 大的元素。

**Complexity:** Time: _O(n log k)_, Space: _O(k)_

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap = []

        for num in nums:
            heapq.heappush(heap, num)
            if len(heap) > k:
                heapq.heappop(heap)
                
        return heap[0]
```

## [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

### **Description**

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,1,1,2,2,3]`, `k = 2`
- **Output**: `[1,2]`

---

**Example 2:**

- **Input**: `nums = [1]`, `k = 1`
- **Output**: `[1]`

---

</details>

### **Solution**

**Idea:** 使用字典来存储每个元素出现的频率，然后使用小顶堆来存储频率最高的元素。`heapq.heappush` 会根据频率 `freq` 排序，因为插入元组时会按元组第一个元素排序。

**Complexity:** Time: _O(n log k)_, Space: _O(n)_

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        freq = {}
        heap = []

        for num in nums:
            freq[num] = freq.get(num, 0) + 1

        for num, freq in freq.items():
            heapq.heappush(heap, (freq, num))
            if len(heap) > k:
                heapq.heappop(heap)
        
        return [heapq.heappop(heap)[1] for _ in range(k)]
```

## 总结 Python 的 heapq 用法

### 1. 什么是 `heapq` ？

`heapq` 是 Python 标准库中的一个模块，提供了堆队列算法的实现。堆是一种特殊的完全二叉树，分为小顶堆和大顶堆。小顶堆的每个节点的值都小于或等于其子节点的值，大顶堆的每个节点的值都大于或等于其子节点的值。`heapq` 只支持小顶堆，如果需要大顶堆，可以插入负值。

### 2. 如何使用 `heapq` ？

#### 2.1 创建堆

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6]
heapq.heapify(nums)  # 让 nums 变成一个最小堆，nums 的值会变成 [1, 1, 2, 3, 5, 9, 4, 6]
```

时间复杂度为 _O(n)_，因为堆化的时间复杂度为 _O(n)_。

或者可以初始化一个空数组，然后使用 `heapq.heappush` 插入元素，也可以创建堆。

#### 2.2 插入元素

```python
heapq.heappush(heap, item)
```

时间复杂度为 _O(log n)_，因为插入元素时需要调整堆。

#### 2.3 获取并删除最小元素

```python
heapq.heappop(heap)
```

时间复杂度为 _O(log n)_，因为删除元素时需要调整堆。

#### 2.4 获取堆顶元素

```python
heap[0]
```

时间复杂度为 _O(1)_，因为堆顶元素就是数组的第一个元素。

#### 2.5 替换堆顶元素

```python
heapq.heapreplace(heap, item)
```

时间复杂度为 _O(log n)_，因为替换元素时需要调整堆。

