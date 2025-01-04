---
title: Leetcode Hashmap Part 1
date: 2025-01-04 10:52:35
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [383. Ransom Note](https://leetcode.com/problems/ransom-note/)

### **Description**

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `ransomNote = "a", magazine = "b"`
- **Output**: `false`

---

**Example 2:**

- **Input**: `ransomNote = "aa", magazine = "ab"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用哈希表来存储 `magazine` 中每个字符出现的次数，然后遍历 `ransomNote` 中的每个字符，如果该字符在哈希表中出现次数大于 0，则将该字符出现次数减 1，否则返回 `false` 。

**Complexity:** Time: _O(n + m)_, Space: _O(n)_ `n` 是 `magazine` 的长度，`m` 是 `ransomNote` 的长度

```python
class Solution(object):
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        count = {}

        for char in magazine:
            count[char] = count.get(char, 0) + 1

        for char in ransomNote:
            if char not in count or count[char] <= 0:
                return False
            count[char] -= 1

        return True
```

## [205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

### **Description**

Given two strings `s` and `t`, determine if they are isomorphic.

Two strings `s` and `t` are isomorphic if the characters in `s` can be replaced to get `t`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "egg", t = "add"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `s = "foo", t = "bar"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用两个哈希表来存储 `s` 到 `t` 和 `t` 到 `s` 的映射关系。如果两个字符串是同构的，那么这两个哈希表应该完全相同。

**Complexity:** Time: _O(n)_, Space: _O(∣Σ∣)_ `Σ` 指的是字符串的字符集。

```python
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        s_to_t = {}
        t_to_s = {}

        for char_s, char_t in zip(s, t):
            if char_s in s_to_t:
                if s_to_t[char_s] != char_t:
                    return False
            else:
                s_to_t[char_s] = char_t

            if char_t in t_to_s:
                if t_to_s[char_t] != char_s:
                    return False
            else:
                t_to_s[char_t] = char_s

        return True
```

## [290. Word Pattern](https://leetcode.com/problems/word-pattern/)

### **Description**

Given a pattern and a string `s`, find if `s` follows the same pattern.

Here, following means a full match, such that there is a bijection between a letter in pattern and a non-empty word in `s`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `pattern = "abba", s = "dog cat cat dog"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `pattern = "abba", s = "dog cat cat fish"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用两个哈希表来存储 `pattern` 到 `s` 和 `s` 到 `pattern` 的映射关系，来检查 `s` 中的每个单词是否都与 `pattern` 中的每个字符一一对应。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def wordPattern(self, pattern, s):
        """
        :type pattern: str
        :type s: str
        :rtype: bool
        """
        words = s.split()
        w_to_p = {}
        p_to_w = {}

        if len(pattern) != len(words):
            return False

        for char, word in zip(pattern, words):
            if char in p_to_w:
                if p_to_w[char] != word:
                    return False
            else:
                p_to_w[char] = word
            if word in w_to_p:
                if w_to_p[word] != char:
                    return False
            else:
                w_to_p[word] = char

        return True
```

## [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

### **Description**

Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "anagram", t = "nagaram"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `s = "rat", t = "car"`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用哈希表来存储 `s` 中每个字符出现的次数，然后遍历 `t` 中的每个字符，如果该字符在哈希表中出现次数大于 0，则将该字符出现次数减 1，否则返回 `false` 。 遍历结束后，检查哈希表中所有字符出现次数是否都为 0，如果都为 0，则返回 `true`，否则返回 `false`。

**Complexity:** Time: _O(n)_, Space: _O(|Σ|)_ `Σ` 指的是字符串的字符集。

```python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        s_dict = {}
        for char in s:
            s_dict[char] = s_dict.get(char, 0) + 1

        for char in t:
            if char not in s_dict:
                return False
            else:
                s_dict[char] -= 1

        all_zeros = all(value == 0 for value in s_dict.values())

        if all_zeros:
            return True
        else:
            return False
```

## [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

### **Description**

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `strs = ["eat","tea","tan","ate","nat","bat"]`
- **Output**: `[["bat"],["nat","tan"],["ate","eat","tea"]]`

---

**Example 2:**

- **Input**: `strs = [""]`
- **Output**: `[[""]]`

---

</details>

### **Solution**

**Idea:** 先对每个字符串进行排序，然后使用哈希表来存储排序后的字符串作为键，将原始字符串作为值。

**Complexity:** Time: _O(n \* k log k)_, Space: _O(n \* k)_ `n` 是字符串的数量，`k` 是字符串的最大长度。

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        strs_dict = defaultdict(list)
        for s in strs:
            key = "".join(sorted(s))
            strs_dict[key].append(s)
        return list(strs_dict.values())
```
