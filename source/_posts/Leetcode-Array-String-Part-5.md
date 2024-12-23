---
title: Leetcode Array & String Part 5
date: 2024-12-22 23:13:13
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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
