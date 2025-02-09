---
title: Leetcode Array & String
date: 2024-12-21 10:52:35
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

### **Description**

Given an array of integers `nums` and an integer `k`, return the total number of subarrays whose sum equals `k`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,1,1], k = 2`
- **Output**: `2`

---

**Example 2:**

- **Input**: `nums = [1,2,3], k = 3`
- **Output**: `2`

---

</details>

### **Solution**

**Idea:** 使用前缀和，遍历数组，计算当前前缀和与 `k` 的差值，如果差值在哈希表中，则说明存在一个子数组，其和为 `k`。

若某个区间 [i, j] 的和为 k，则满足 `pre_sum[j] - pre_sum[i - 1] = k`，即 `pre_sum[j] - k = pre_sum[i - 1]`。

**Complexity:** Time: _O(n)_, Space: _O(n)_
```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        pre_sum = {0: 1} # 初始化前缀和为 0 的计数为 1，key 为前缀和，value 为计数
        cur_sum = 0
        count = 0

        for num in nums:
            cur_sum += num
            if cur_sum - k in pre_sum:
                count += pre_sum[cur_sum - k]
            pre_sum[cur_sum] = pre_sum.get(cur_sum, 0) + 1
        
        return count
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

## [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

### **Description**

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1, 2, 3, 4, 5, 6, 7], k = 3`
- **Output**: `[5, 6, 7, 1, 2, 3, 4]`

---

**Example 2:**

- **Input**: `nums = [-1, -100, 3, 99], k = 2`
- **Output**: `[3, 99, -1, -100]`

---

</details>

### **Solution**

#### **Approach 1** 切片

**Idea:** 这道题的本质是数组旋转，直觉上很容易想到将数组分成两部分，然后拼接起来。但这样需要额外空间，不是最优解。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def rotate(self, nums, k):
        n = len(nums)
        k = k % n
        nums[:] = nums[-k:] + nums[:-k]
```

#### **Approach 2** 数组翻转

**Idea:** 可以通过三次翻转来实现题目要求，首先将整个数组翻转，然后翻转前 `k` 个元素，最后翻转剩下的元素。

![rotate-array](/img/leetcode189.jpg)

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def rotate(self, nums, k):
        def reverse(i, j):
            while i < j:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                j -= 1

        n = len(nums)
        k = k % n
        reverse(0, n - 1)
        reverse(0, k - 1)
        reverse(k, n - 1)
```

## [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

### **Description**

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is guaranteed to fit in a 32-bit integer.

You must write an algorithm that runs in _O(n)_ time and without using the division operation.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1, 2, 3, 4]`
- **Output**: `[24, 12, 8, 6]`

---

**Example 2:**

- **Input**: `nums = [-1, 1, 0, -3, 3]`
- **Output**: `[0, 0, 9, 0, 0]`

</details>

### **Solution**

#### **Approach 1:** 前缀积和后缀积

**Idea:** 我们不必将所有数字的乘积除以给定索引处的数字得到相应的答案，而是利用索引 `左侧所有数字的乘积` 和 `右侧所有数字的乘积`（即前缀与后缀）相乘得到答案。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        n = len(nums)
        L, R, answer = [0]*n, [0]*n, [0]*n

        L[0] = 1
        for i in range(1, n):
            L[i] = nums[i - 1] * L[i - 1]

        R[n - 1] = 1
        for i in range(i - 1, -1, -1):
            R[i] = nums[i + 1] * R[i + 1]

        for i in range(n):
            answer[i] = L[i] * R[i]

        return answer
```

#### **Approach 2:** 空间优化

**Idea:** 我们可以直接将答案数组作为前缀数组，这样我们就可以省略额外的空间。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def productExceptSelf(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        n = len(nums)
        answer = [0] * n

        answer[0] = 1
        for i in range(1, n):
            answer[i] = answer[i - 1] * nums[i - 1]

        R = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= R
            R *= nums[i]

        return answer
```

---

`Top Interview 150 补充`

---

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

#### **Approach 1:** 哈希表

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

#### **Approach 2:** 摩尔投票法

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

## [274. H-Index](https://leetcode.com/problems/h-index/)

### **Description**

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their ith paper, return the researcher's h-index.

According to the definition of h-index on Wikipedia: A scientist has an index `h` if `h` of their `n` papers have at least `h` citations each, and the other `n - h` papers have no more than `h` citations each.

If there are several possible values for `h`, the maximum one is taken as the h-index.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `citations = [3, 0, 6, 1, 5]`
- **Output**: `3`

---

**Example 2:**

- **Input**: `citations = [1, 3, 1]`
- **Output**: `1`

</details>

### **Solution**

#### **Approach 1:** 排序

**Idea:** 将数组从大到小排序后，遍历找到第一个满足条件的 `h`。根据 H 指数的定义，如果当前 H 指数为 `h` 并且在遍历过程中找到当前值 `citations[i] > h`，则说明我们**找到了一篇被引用了至少 `h+1` 次的论文**，所以将现有的 `h` 值加 1。继续遍历直到 `h` 无法继续增大。最后返回 `h` 作为最终答案。

**Complexity:** Time: _O(n log n)_, Space: _O(log n)_, 即为排序的时间和空间复杂度。

```python
class Solution(object):
    def hIndex(self, citations):
        """
        :type citations: List[int]
        :rtype: int
        """
        citations.sort(reverse=True)
        h = 0
        i = 0
        while i < len(citations) and citations[i] >= h + 1:
            h += 1
            i += 1
        return h
```

#### **Approach 2:** 计数排序

**Idea:** 根据上述解法我们发现，最终的时间复杂度与排序算法的时间复杂度有关，所以我们可以使用计数排序算法，新建并维护一个数组 `counter` 用来记录当前引用次数的论文有几篇。

根据定义，我们可以发现 H 指数不可能大于总的论文发表数，所以对于引用次数超过论文发表数的情况，我们可以将其按照总的论文发表数来计算即可。这样我们可以限制参与排序的数的大小为 `[0,n]`（其中 `n` 为总的论文发表数），使得计数排序的时间复杂度降低到 `O(n)`。

最后我们可以从后向前遍历数组 `counter`，对于每个 `0 ≤ i ≤ n`，在数组 `counter` 中得到大于或等于当前引用次数 `i` 的总论文数。当我们找到一个 H 指数时跳出循环，并返回结果。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def hIndex(self, citations):
        """
        :type citations: List[int]
        :rtype: int
        """
        n = len(citations)
        total = 0
        counter = [0] * (n + 1)
        for c in citations:
            if c >= n:
                counter[n] += 1
            else:
                counter[c] += 1
        for i in range(n, -1, -1):
            total += counter[i]
            if total >= i:
                return i
        return 0
```

#### **Approach 3:** 二分查找

**Idea:** 我们需要找到一个值 `h`，它是满足有 `h` 篇论文的引用次数至少为 `h` 的 **最大值**。小于等于 `h` 的所有值 `x` 都满足这个性质，而大于 `h` 的值都不满足这个性质。所以这个问题可以用二分搜索来解决。

**Complexity:** Time: _O(log n)_, Space: _O(1)_

```python
class Solution(object):
    def hIndex(self, citations):
        """
        :type citations: List[int]
        :rtype: int
        """
        left = 0
        right = len(citations)
        while left <= right:
            mid = (left + right) // 2
            count = 0
            for c in citations:
                if c >= mid:
                    count += 1
            if count >= mid:
                left = mid + 1
            else:
                right = mid - 1
        return left - 1
```

## [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)

### **Description**

Implement the `RandomizedSet` class:

- `RandomizedSet()` Initializes the `RandomizedSet` object.
- `bool insert(int val)` Inserts an item `val` into the set if not present, and returns `true` if the item was not present.
- `bool remove(int val)` Removes an item `val` from the set if present, and returns `true` if the item was present.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.

You must implement the functions of the class such that each function works in average O(1) time complexity.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `["RandomizedSet", "insert 1", "remove 2", "insert 2", "getRandom", "remove 1", "insert 2", "getRandom"]`
- **Output**: `[null, true, false, true, 2, true, false, 2]`

---

</details>

### **Solution**

**Idea:** 变长数组 + 哈希表。

变长数组可以在 _O(1)_ 的时间内完成获取随机元素操作，但是由于无法在 _O(1)_ 的时间内判断元素是否存在，因此不能在 _O(1)_ 的时间内完成插入和删除操作。

哈希表可以在 _O(1)_ 的时间内完成插入和删除操作，但是由于无法根据下标定位到特定元素，因此不能在 _O(1)_ 的时间内完成获取随机元素操作。

为了满足插入、删除和获取随机元素操作的时间复杂度都是 _O(1)_ ，需要将变长数组和哈希表结合，变长数组中存储元素，哈希表中存储每个元素在变长数组中的下标。哈希表存储元素的下标是为了确保删除的复杂度是 _O(1)_ 。删除时将变长数组的最后一个元素移动到待删除元素的下标处，然后删除变长数组的最后一个元素。

**Complexity:** Time: _O(1)_, Space: _O(n)_

```python
import random

class RandomizedSet(object):

    def __init__(self):
        self.nums_arr = []
        self.nums_dict = {}

    def insert(self, val):
        """
        :type val: int
        :rtype: bool
        """
        if val in self.nums_arr:
            return False
        else:
            self.nums_arr.append(val)
            self.nums_dict[val] = len(self.nums_arr) - 1
            return True

    def remove(self, val):
        """
        :type val: int
        :rtype: bool
        """
        if val not in self.nums_arr:
            return False
        else:
            index = self.nums_dict[val]
            self.nums_arr[index] = self.nums_arr[-1]
            self.nums_dict[self.nums_arr[index]] = index
            self.nums_arr.pop()
            del self.nums_dict[val]
            return True

    def getRandom(self):
        """
        :rtype: int
        """
        return random.choice(self.nums_arr)
```

## [134. Gas Station](https://leetcode.com/problems/gas-station/)

### **Description**

There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.

You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return `-1`. If there exists a solution, it is guaranteed to be unique.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `gas = [1, 2, 3, 4, 5], cost = [3, 4, 5, 1, 2]`
- **Output**: `3`

---

**Example 2:**

- **Input**: `gas = [2, 3, 4], cost = [3, 4, 3]`
- **Output**: `-1`

</details>

### **Solution**

**Idea:** 我们可以将问题转化为一个折线图问题，其中 `gas[i]` 表示在第 `i` 个加油站的油量，`cost[i]` 表示从第 `i` 个加油站到第 `i+1` 个加油站的油量消耗。我们需要找到一个起点，使得从该起点出发，能够绕一圈回到起点，并且油量不会耗尽。

![gas-station](/img/leetcode134.png)

如果 `gas` 元素和大于等于 `cost` 元素和，则一定存在一个起点，使得从该起点出发，能够绕一圈回到起点，并且油量不会耗尽。该起点即为折线图中油量最低点（可以想象最低点和 X 轴的高度一致，则从最低点开始，油量一定不会耗尽）。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        answer = 0
        quantity = 0
        min_quantity = 0
        for i, (g, c) in enumerate(zip(gas, cost)):
            quantity += g - c
            if quantity < min_quantity:
                min_quantity = quantity
                answer = i + 1
        return answer if quantity >= 0 else -1
```

## [135. Candy](https://leetcode.com/problems/candy/)

### **Description**

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute to the children.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `ratings = [1, 0, 2]`
- **Output**: `5`

---

**Example 2:**

- **Input**: `ratings = [1, 2, 2]`
- **Output**: `4`

</details>

### **Solution**

#### **Approach 1:** 左右遍历

**Idea:** 我们可以从左到右遍历一次，确保每个孩子都比左边的孩子多一个糖果。然后从右到左遍历一次，确保每个孩子都比右边的孩子多一个糖果。最后遍历数组取最大值，就会同时满足左规则和右规则。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def candy(self, ratings):
        """
        :type ratings: List[int]
        :rtype: int
        """
        n = len(ratings)
        left = [1] * n
        for i in range(1, n):
            if ratings[i] > ratings[i - 1]:
                left[i] = left[i - 1] + 1
            else:
                left[i] = 1

        right = [1] * n
        for i in range (n - 2, -1, -1):
            if ratings[i] > ratings[i + 1]:
                right[i] = right[i + 1] + 1
            else:
                right[i] = 1

        answer = 0
        for i in range (n):
            answer += max(left[i], right[i])

        return answer
```

#### **Approach 2:** 常数空间遍历

**Complexity:** Time: _O(n)_, Space: _O(1)_

这种解法比较难想，且不具有通用性，所以这里不做详细介绍。详细解法可以参考 [官方题解](https://leetcode.cn/problems/candy/solutions/533150/fen-fa-tang-guo-by-leetcode-solution-f01p/)。

## [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

### **Description**

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "III"`
- **Output**: `3`

---

**Example 2:**

- **Input**: `s = "LVIII"`
- **Output**: `58`

---

**Example 3:**

- **Input**: `s = "MCMXCIV"`
- **Output**: `1994`

---

</details>

### **Solution**

**Idea:** 使用一个字典来存储罗马数字和整数之间的映射关系，然后遍历字符串，根据当前字符和下一个字符的值来决定是加还是减。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        SYMBOL_VALUES = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000
        }
        ans = 0
        for i, roman in enumerate(s):
            value = SYMBOL_VALUES[roman]
            if i < len(s) - 1 and value < SYMBOL_VALUES[s[i + 1]]:
                ans -= value
            else:
                ans += value
        return ans
```

## [12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

### **Description**

Seven different symbols represent Roman numerals with the following values:

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

- If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
- If the value starts with 4 or 9 use the **subtractive form** representing one symbol subtracted from the following symbol, for example, 4 is 1 (`I`) less than 5 (`V`): `IV` and 9 is 1 (`I`) less than 10 (`X`): `IX`. Only the following subtractive forms are used: `4` (IV), `9` (IX), `40` (XL), `90` (XC), `400` (CD) and `900` (CM).
- Only powers of 10 (`I`, `X`, `C`, `M`) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (`V`), 50 (`L`), or 500 (`D`) multiple times. If you need to append a symbol 4 times use the **subtractive form**.

Given an integer, convert it to a Roman numeral.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `3749`
- **Output**: `MMMDCCXLIX`

---

**Example 2:**

- **Input**: `1994`
- **Output**: `MCMXCIV`

---

</details>

### **Solution**

**Idea:** 从大到小依次处理千位、百位、十位和个位，根据当前位数的值选择合适的罗马数字符号并进行拼接。对 4 和 9 进行特殊处理。用一个变量`remain`来存储剩余的值，并逐步减去相应的值。

**Complexity:** Time: _O(1)_, Space: _O(1)_ 时间复杂度是常数，因为罗马数字的表示是有限的（1-3999）。

```python
class Solution(object):
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        roman = ''

        num_M = num // 1000
        roman += 'M' * num_M
        remain = num - num_M * 1000

        if remain // 100 == 9:
            roman += 'CM'
            remain -= 900
        elif remain // 100 == 4:
            roman += 'CD'
            remain -= 400
        else:
            num_D = remain // 500
            roman += 'D' * num_D
            remain -= num_D * 500
            num_C = remain // 100
            roman += 'C' * num_C
            remain -= num_C * 100

        if remain // 10 == 9:
            roman += 'XC'
            remain -= 90
        elif remain // 10 == 4:
            roman += 'XL'
            remain -= 40
        else:
            num_L = remain // 50
            roman += 'L' * num_L
            remain -= num_L * 50
            num_X = remain // 10
            roman += 'X' * num_X
            remain -= num_X * 10

        if remain // 1 == 9:
            roman += 'IX'
            remain -= 9
        elif remain // 1 == 4:
            roman += 'IV'
            remain -= 4
        else:
            num_V = remain // 5
            roman += 'V' * num_V
            remain -= num_V * 5
            num_I = remain // 1
            roman += 'I' * num_I

        return roman
```

## [58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)

### **Description**

Given a string `s` consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "Hello World"`
- **Output**: `5`

---

**Example 2:**

- **Input**: `s = "   fly me   to   the moon  "`
- **Output**: `4`

---

</details>

### **Solution**

**Idea:** 从后往前遍历字符串，找到最后一个单词的长度。用`start`来标记是否已经找到第一个非空字符。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        length = 0
        start = False

        for letter in reversed(s):
            if letter != ' ':
                length += 1
                start = True
            else:
                if start:
                    return length

        return length
```

## [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

### **Description**

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `["flower","flow","flight"]`
- **Output**: `"fl"`

---

**Example 2:**

- **Input**: `["dog","racecar","car"]`
- **Output**: `""`

---

</details>

### **Solution**

**Idea:** 将第一个字符串假设为公共前缀，然后逐步检查它是否是所有字符串的公共前缀。如果不是，则将第一个字符串的长度减一，直到找到公共前缀。

**Complexity:** Time: _O(mn)_, Space: _O(1)_ 时间复杂度是 _O(mn)_ 是因为每次比较两个字符串需要 _O(n)_ 的时间，而需要比较 _O(m)_ 次。`m` 是字符串的平均长度，`n` 是字符串的数量。

```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        pref = strs[0]
        pref_len = len(pref)

        for s in strs[1:]:
            while pref != s[0:pref_len]:
                pref_len -= 1
                if pref_len == 0:
                    return ''
                pref = pref[0:pref_len]

        return pref
```

## [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

### **Description**

Given an input string `s`, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in `s` will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "the sky is blue"`
- **Output**: `"blue is sky the"`

---

**Example 2:**

- **Input**: `s = "  hello world  "`
- **Output**: `"world hello"`

---

</details>

### **Solution**

**Idea:** 从后往前遍历字符串，将单词逐个添加到结果字符串中，并确保每个单词之间只有一个空格。最后检查是否还有没处理完的单词，如果有，则将其添加到结果字符串中。并且检查结果字符串的最后一个字符是否是空格，如果是，则将其删除。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        ans = ''
        word = ''

        for letter in reversed(s):
            if letter != ' ':
                word = letter + word
            else:
                if word:
                    ans += word + ' '
                    word = ''

        if word:
            ans += word

        if ans[-1] == ' ':
            ans = ans[:-1]

        return ans
```

## [6. Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

### **Description**

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "PAYPALISHIRING", numRows = 3`
- **Output**: `"PAHNAPLSIIGYIR"`

---

**Example 2:**

- **Input**: `s = "PAYPALISHIRING", numRows = 4`
- **Output**: `"PINALSIGYAHRPI"`

---

</details>

### **Solution**

**Idea:** 将字符串按行分组，然后按行输出。用`index`来标记当前行，用`increasing`来标记当前行是正在增加还是减少。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
class Solution(object):
    def convert(self, s, numRows):
        """
        :type s: str
        :type numRows: int
        :rtype: str
        """
        if numRows == 1:
            return s

        rows = [''] * numRows
        index = 0
        increasing = True

        for i, letter in enumerate(s):
            if index == numRows - 1:
                increasing = False
            elif i != 0 and index == 0:
                increasing = True

            if increasing:
                rows[index] += letter
                index += 1
            else:
                rows[index] += letter
                index -= 1

        return ''.join(rows)
```

## [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

### **Description**

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `haystack = "sadbutsad", needle = "sad"`
- **Output**: `0`

---

**Example 2:**

- **Input**: `haystack = "leetcode", needle = "leeto"`
- **Output**: `-1`

---

</details>

### **Solution**

#### **Approach 1:** 暴力匹配

**Idea:** 让字符串 `needle` 与字符串 `haystack` 的所有长度为 `m` 的子串均匹配一次。

**Complexity:** Time: _O(mn)_, Space: _O(1)_

```python
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        for i in range(0,len(haystack)-len(needle)+1):
            if haystack[i:i+len(needle)] == needle:
                return i
        return -1
```

#### **Approach 2:** KMP 算法

**Idea:** 首先构建部分匹配表（`lps` 数组），`lps[i]` 表示 `needle[0:i+1]` 的最长公共前后缀的长度，构建 `lps` 的时间复杂度为 _O(n)_。然后遍历 `haystack`，用 `lps` 数组来跳过不匹配的字符，时间复杂度为 _O(m)_。

这个方法理解起来有点复杂，我们举一个具体的例子来说明。

**输入：** `haystack = "ababcabcabababd"`, `needle = "ababd"`

**步骤 1：构建 `lps` 数组**

（1）初始化 `lps` 数组，长度与 `needle` 相同，全为 0。

（2）遍历 `needle[1:]`, 计算 `lps` 数组。

- `i = 1`, 字符 `b != a`, `j = 0` → `lps[1] = 0`。
- `i = 2`, 字符 `a == a`, `j = 1` → `lps[2] = 1`。
- `i = 3`, 字符 `b == b`, `j = 2` → `lps[3] = 2`。
- `i = 4`, 字符 `d != a`, `j = 0` → `lps[4] = 0`。

| index  | 0   | 1   | 2   | 3   | 4   |
| ------ | --- | --- | --- | --- | --- |
| needle | a   | b   | a   | b   | d   |
| lps    | 0   | 0   | 1   | 2   | 0   |

**步骤 2：匹配 `haystack` 和 `needle`**

（1）初始化 `i = 0`, `j = 0`，表示当前匹配到 `haystack` 和 `needle` 的位置。

（2）遍历 `haystack`，用 `lps` 数组来跳过不匹配的字符。

- `i = 0`, `j = 0`, `haystack[0] == needle[0]` → `j = 1`, `i = 1`
- `i = 1`, `j = 1`, `haystack[1] == needle[1]` → `j = 2`, `i = 2`
- `i = 2`, `j = 2`, `haystack[2] == needle[2]` → `j = 3`, `i = 3`
- `i = 3`, `j = 3`, `haystack[3] == needle[3]` → `j = 4`, `i = 4`
- `i = 4`, `j = 4`, `haystack[4] != needle[4]` → 回退 `j = lps[3] = 2`
- `i = 4`, `j = 2`, `haystack[4] == needle[2]` → `j = 3`, `i = 5`
- ...

最终在 `i = 14`, `j = 5` 时，`haystack[10:15] == needle`，返回 `i - len(needle) + 1 = 10`。

**Complexity:** Time: _O(n+m)_, Space: _O(n)_ `n` 是 `needle` 的长度，`m` 是 `haystack` 的长度。

**加速的原因：**

- **暴力匹配：**每次匹配失败后，`needle` 的指针回到开头，`haystack` 的指针回退，逐个字符重新匹配。
- **KMP 算法：**利用部分匹配表 `lps` 跳过部分字符， `needle` 的指针跳转到部分匹配的位置，`haystack` 的指针继续前进。

```python
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        if not needle:
            return 0
        if not haystack or len(needle) > len(haystack):
            return -1

        # 构建 lps 数组（部分匹配表）
        def build_lps(pattern):
            lps = [0] * len(pattern)
            j = 0  # 记录最长相等前后缀的长度
            for i in range(1, len(pattern)):
                while j > 0 and pattern[i] != pattern[j]:
                    j = lps[j - 1]  # 回退到之前匹配的位置
                if pattern[i] == pattern[j]:
                    j += 1
                lps[i] = j
            return lps

        lps = build_lps(needle)

        # 开始匹配 haystack 和 needle
        j = 0  # 指针 j 表示当前匹配到 needle 的位置
        for i in range(len(haystack)):
            while j > 0 and haystack[i] != needle[j]:
                j = lps[j - 1]  # 回退到 lps 的值
            if haystack[i] == needle[j]:
                j += 1
            if j == len(needle):  # 完全匹配
                return i - len(needle) + 1

        return -1  # 如果没有匹配到
```

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
