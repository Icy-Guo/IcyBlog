---
title: Leetcode Hashmap
date: 2024-12-11 10:52:35
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [1. Two Sum](https://leetcode.com/problems/two-sum/)

### **Description**

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [2,7,11,15], target = 9`
- **Output**: `[0,1]`

---

**Example 2:**

- **Input**: `nums = [3,2,4], target = 6`
- **Output**: `[1,2]`

---

</details>

### **Solution**

**Idea:** 使用哈希表来存储 `nums` 中每个元素的 `值` 和它的 `索引`，然后遍历数组，对于每个元素，检查哈希表中是否存在 `target - num` 的值，如果存在，则返回这两个值的索引，否则将该元素的值和它的索引存入哈希表中。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        index = {}
        for i, num in enumerate(nums):
            if target - num in index:
                return[i, index[target - num]]
            else:
                index[num] = i
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

## [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

### **Description**

Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [100,4,200,1,3,2]`
- **Output**: `4`

---

**Example 2:**

- **Input**: `nums = [0,3,7,2,5,8,4,6,0,1]`
- **Output**: `9`

---

</details>

### **Solution**

**Idea:** 对于匹配的过程，暴力的方法是 _O(n)_ 遍历数组去看是否存在这个数，更高效的方法是用一个哈希集合存储数组中的数，这样查看一个数是否存在即能优化至 _O(1)_ 的时间复杂度。

但是仅仅是这样，我们的算法时间复杂度最坏情况下还是会达到 _O(n ^ 2)_（即外层需要枚举 _O(n)_ 个数，内层需要暴力匹配 _O(n)_ 次），无法满足题目的要求。但分析这个过程，我们会发现其中执行了很多不必要的枚举，如果已知有一个 `x, x+1, x+2, ⋯, x+y` 的连续序列，那么我们不需要从 `x+1, x+2, ⋯, x+y` 处开始尝试匹配，而是直接从 `x` 开始尝试匹配。

这样，外层循环需要 _O(n)_ 的时间复杂度，只有当一个数是连续序列的第一个数的情况下才会进入内层循环，然后在内层循环中匹配连续序列中的数，因此数组中的每个数只会进入内层循环一次。所以，总的时间复杂度为 _O(n)_。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        nums_set = set(nums)
        ans = 0

        for num in nums_set:
            if num - 1 not in nums_set:
                cur_num = num
                length = 1

                while cur_num + 1 in nums_set:
                    cur_num += 1
                    length += 1

                ans = max(ans, length)

        return ans
```

---

`Top Interview 150 补充`

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

## [202. Happy Number](https://leetcode.com/problems/happy-number/)

### **Description**

Write an algorithm to determine if a number `n` is happy.

A happy number is a number defined by the following process:

1. Starting with any positive integer, replace the number by the sum of the squares of its digits.
2. Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
3. Those numbers for which this process ends in 1 are happy.

Return `true` if `n` is a happy number, and `false` if not.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `n = 19`
- **Output**: `true`
- **Explanation**:
  - 1^2 + 9^2 = 1 + 81 = 82
  - 8^2 + 2^2 = 64 + 4 = 68
  - 6^2 + 8^2 = 36 + 64 = 100
  - 1^2 + 0^2 + 0^2 = 1 + 0 + 0 = 1

---

**Example 2:**

- **Input**: `n = 2`
- **Output**: `false`

---

</details>

### **Solution**

#### **Approach 1:** 哈希集合检查循环

**Idea:** 使用哈希集合来存储已经出现过的数字，如果出现重复的数字，则说明出现了循环，返回 `false`。一直循环直到 `n` 等于 1 或者出现循环。

**Complexity:** Time: _O(log n)_, Space: _O(log n)_ `n` 是 `n` 的位数

```python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        def get_next(n):
            total_sum = 0
            while n > 0:
                s = n % 10
                total_sum += s ** 2
                n = n // 10
            return total_sum

        exist = set()
        while n not in exist:
            if n == 1:
                return True
            exist.add(n)
            n = get_next(n)
        return False
```

#### **Approach 2:** 快慢指针检查循环

**Idea:** 反复调用 `get_next` 函数得到的其实是一个隐式的链表。那么这个问题就可以转换为检测一个链表是否有环。可以使用快慢指针来检查循环，快指针每次走两步，慢指针每次走一步。如果快慢指针相遇，则说明出现了循环，返回 `false`，否则返回 `true`。

**Complexity:** Time: _O(log n)_, Space: _O(1)_ `n` 是 `n` 的位数

```python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        def get_next(n):
            total_sum = 0
            while n > 0:
                s = n % 10
                total_sum += s ** 2
                n = n // 10
            return total_sum

        slow_runner = n
        fast_runner = get_next(n)
        while fast_runner != 1 and slow_runner != fast_runner:
            slow_runner = get_next(slow_runner)
            fast_runner = get_next(get_next(fast_runner))
        return fast_runner == 1
```

## [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

### **Description**

Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [1,2,3,1], k = 3`
- **Output**: `true`

---

**Example 2:**

- **Input**: `nums = [1,0,1,1], k = 1`
- **Output**: `true`

---

</details>

### **Solution**

#### **Approach 1:** 哈希表

**Idea:** 使用哈希表来存储 `nums` 中每个元素的 `值` 和它的 `索引`，然后遍历数组，对于每个元素，检查哈希表中是否存在 `nums[i]` 的值，如果存在，则检查 `(i - index[nums[i]]) <= k`，如果满足条件，则返回 `true`，否则将该元素的值和它的索引存入哈希表中。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        index = {}
        for i, num in enumerate(nums):
            if num in index and (i - index[num]) <= k:
                return True
            else:
                index[num] = i
        return False
```

#### **Approach 2:** 滑动窗口

**Idea:** 使用滑动窗口来检查 `nums` 中是否存在两个相同的元素。如果当前窗口大小大于 `k`，则移除窗口最左边的元素，否则将当前元素加入窗口。如果窗口中存在相同的元素，则返回 `true`，否则返回 `false`。

**Complexity:** Time: _O(n)_, Space: _O(k)_

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        s = set()
        for i, num in enumerate(nums):
            if i > k:
                s.remove(nums[i - k - 1])
            if num in s:
                return True
            s.add(num)
        return False
```
