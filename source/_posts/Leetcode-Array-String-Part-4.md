---
title: Leetcode Array & String Part 4
date: 2024-12-20 10:40:06
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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
