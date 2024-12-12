---
title: Leetcode Array & String Part 2
date: 2024-12-12 10:14:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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
class Solution:
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
class Solution:
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

#### **Approach** 动态规划

**Idea:** 这道题的本质是寻找数组中的最大差值。思路可以总结为如果某天卖出股票，则该天卖出的最大利润为(该天的股价-前面天数中最小的股价)，然后与已知的最大利润比较，如果大于则更新当前最大利润的值。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        max_profit = 0
        buy_price = prices[0]

        for price in prices[1:]:
            if price < buy_price:
                buy_price = price
            else:
                max_profit = max(max_profit, price - buy_price)

        return max_profit
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

#### **Approach** 贪心算法

**Idea:** 贪心算法指每一步都做出局部最优解，从而达到全局最优解。这道题中具体指：所有上涨的交易日都进行交易（赚到所有可能的利润），所有下降的交易日都不进行交易（永远不亏钱）。这样这道题就可以简化为遍历数组，如果当前天数的价格大于前一天的价格，则将差值累加到最大利润中。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
class Solution:
    def maxProfit(self, prices):
        max_profit = 0
        for i in range(1, len(prices)):
            max_profit += max(0, prices[i] - prices[i - 1])
        return max_profit
```
