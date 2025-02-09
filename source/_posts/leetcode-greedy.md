---
title: Leetcode Greedy Algorithm
date: 2025-02-08 15:38:35
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### **Description**

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `prices = [7, 1, 5, 3, 6, 4]`
- **Output**: `5`

---

**Example 2:**

- **Input**: `prices = [7, 6, 4, 3, 1]`
- **Output**: `0`

---

</details>

### **Solution**

**Idea:** 动态规划。这道题的本质是寻找数组中的最大差值。思路可以总结为如果某天卖出股票，则该天卖出的**最大利润**为**该天的股价-前面天数中最小的股价**，然后与已知的最大利润比较，如果大于则更新当前最大利润的值。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def maxProfit(self, prices):
        max_profit = 0
        buy_price = prices[0]

        for price in prices[1:]:
            if price < buy_price:
                buy_price = price
            else:
                max_profit = max(max_profit, price - buy_price)

        return max_profit
```

## [55. Jump Game](https://leetcode.com/problems/jump-game/)

### **Description**

You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [2, 3, 1, 1, 4]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `nums = [3, 2, 1, 0, 4]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 这道题的本质是判断数组中是否存在一个路径可以到达最后一个元素。可以采用贪心算法，不断更新能到达的最远位置。如果当前位置大于能到达的最远位置，则返回 `false`，否则更新能到达的最远位置。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def canJump(self, nums):
        max_length = 0

        for i, jump in enumerate(nums):
            if i <= max_length:
                max_length = max (max_length, i + jump)
            else:
                return False

        return True
```

## [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

### **Description**

You are given a 0-indexed array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]`
- `i + j < n`

Return the minimum number of jumps to reach `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [2, 3, 1, 1, 4]`
- **Output**: `2`

---

**Example 2:**

- **Input**: `nums = [2, 3, 0, 1, 4]`
- **Output**: `2`

---

</details>

### **Solution**

**Idea:** 这道题的本质是寻找数组中到达最后一个元素的最小跳跃次数。可以采用贪心算法，将问题想象成搭桥过河，在可以选择的桥中，选择**右端点最远**的桥。本题的思路有些难懂，需要仔细思考每个变量的含义。

![leetcode45](/img/leetcode45.png)

这里要注意一个细节，循环时 `i` 只循环到 `n-2`。因为开始的时候边界是第 0 个位置，`steps` 已经加 1 了，所以假如最后一步从 `n-2` 跳到 `n-1`，那么 `steps` 就不需要再加 1 了。所以如果 `i` 循环到 `n-1`，那么 `steps` 就会额外再加 1 导致出错。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def jump(self, nums):
        max_length, cur_right, step = 0, 0, 0

        for i in range(len(nums) - 1):
            if i <= max_length:
                max_length = max (max_length, i + nums[i])
                if i == cur_right:
                    cur_right = max_length
                    step += 1

        return step
```

## [763. Partition Labels](https://leetcode.com/problems/partition-labels/)

### **Description**

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return a list of integers representing the size of these parts.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "ababcbacadefegdehijhklij"`
- **Output**: `[9, 7, 8]`

---

**Example 2:**

- **Input**: `s = "eccbbbbdec"`
- **Output**: `[10]`

---

</details>

### **Solution**

**Idea:** 这道题的本质是寻找字符串中每个字符的最后一个位置，然后根据每个字符的最后一个位置将字符串分割成若干个部分。分隔时，`end` 是当前字符的最后一个位置，`start` 是当前字符的开始位置。当 `i` 等于 `end` 时，说明当前字符的最后一个位置已经确定，将 `end - start + 1` 加入结果列表，并更新 `start` 为 `i + 1`。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        last_index = {}
        for i, c in enumerate(s):
            last_index[c] = i

        result = []
        start = 0
        end = 0

        for i, c in enumerate(s):
            end = max(end, last_index[c])
            if i == end:
                result.append(end - start + 1)
                start = i + 1

        return result
```

---

`Top Interview 150 补充`

---

## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

### **Description**

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
- **Output**: `6`

---

**Example 2:**

- **Input**: `nums = [1]`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用贪心算法，要么继续累加当前元素，要么从当前元素开始重新累加。计算包含当前元素的最大连续子数组和 `cur_sum`，如果 `cur_sum` 小于当前元素，则更新 `cur_sum` 为当前元素，否则将当前元素加入 `cur_sum`。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = float("-inf")
        cur_sum = 0

        for num in nums:
            cur_sum = max(cur_sum + num, num)
            max_sum = max(max_sum, cur_sum)
        
        return max_sum
```

## [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

### **Description**

You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one share** of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return the **maximum profit** you can achieve.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `prices = [7, 1, 5, 3, 6, 4]`
- **Output**: `7`

---

**Example 2:**

- **Input**: `prices = [1, 2, 3, 4, 5]`
- **Output**: `4`

---

**Example 3:**

- **Input**: `prices = [7, 6, 4, 3, 1]`
- **Output**: `0`

---

</details>

### **Solution**

**Idea:** 贪心算法指每一步都做出局部最优解，从而达到全局最优解。这道题中具体指：**所有上涨的交易日都进行交易（赚到所有可能的利润），所有下降的交易日都不进行交易（永远不亏钱）**。这样这道题就可以简化为遍历数组，如果当前天数的价格大于前一天的价格，则将差值累加到最大利润中。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution(object):
    def maxProfit(self, prices):
        max_profit = 0
        for i in range(1, len(prices)):
            max_profit += max(0, prices[i] - prices[i - 1])
        return max_profit
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
