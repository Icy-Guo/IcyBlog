---
title: Leetcode Array & String Part 3
date: 2024-12-17 22:36:00
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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
