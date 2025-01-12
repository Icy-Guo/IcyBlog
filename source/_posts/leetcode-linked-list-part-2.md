---
title: leetcode-linked-list-part-2
date: 2025-01-11 11:46:19
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

### **Description**

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,4,5], n = 2`
- **Output**: `[1,2,3,5]`

---

**Example 2:**

- **Input**: `head = [1], n = 1`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 要删除倒数第n个节点，我们需要知道它的前一个节点。可以先遍历一遍链表，找到链表的长度，然后从链表的头部开始，找到需要删除的节点的前一个节点，对需要删除的节点进行删除。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        def getLength(head: ListNode) -> int:
            length = 0
            while head:
                length += 1
                head = head.next
            return length
    
        length = getLength(head)
        count = 0
        dummy_node = ListNode(0, head)
        current = dummy_node
        for _ in range(length - n):
            current = current.next
        current.next = current.next.next
        return dummy_node.next
```

## [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

### **Description**

Given the `head` of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,3,4,4,5]`
- **Output**: `[1,2,5]`

---

**Example 2:**

- **Input**: `head = [1,1,1,2,3]`
- **Output**: `[2,3]`

---

</details>

### **Solution**

**Idea:** 遍历链表，如果 **当前节点的下一个节点** 和 **下一个节点的下一个节点** 的值相同，则删除当前节点之后的节点，直到找到下一个不重复的节点。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy_node = ListNode(0, head)
        current = dummy_node
        while current.next and current.next.next:
            if current.next.val == current.next.next.val:
                value = current.next.val
                while current.next and current.next.val == value:
                    current.next = current.next.next
            else:
                current = current.next
        return dummy_node.next
```

## [61. Rotate List](https://leetcode.com/problems/rotate-list/)

### **Description**

Given the `head` of a linked list, rotate the list to the right by `k` places.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,4,5], k = 2`
- **Output**: `[4,5,1,2,3]`

---

**Example 2:**

- **Input**: `head = [0,1,2], k = 4`
- **Output**: `[2,0,1]`

---

</details>

### **Solution**

**Idea:** 这道题的输入 `k` 可能大于链表的长度，为了不超时，需要先计算链表的长度，然后取模得到真正需要旋转的次数。每次旋转时，找到 **倒数第二个节点**，将 **最后一个节点** 插入到 **第一个节点** 之前，并更新 `head` 为最新的头节点。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def rotateRight(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        if k == 0 or not head or not head.next:
            return head

        length = 0 
        current = head
        while current:
            current = current.next
            length += 1
        k = k % length

        dummy_node = ListNode(0, head)     
        for _ in range(k):
            last_2_node = head
            while last_2_node.next.next:
                last_2_node = last_2_node.next
            dummy_node.next = last_2_node.next
            last_2_node.next = None
            dummy_node.next.next = head
            head = dummy_node.next
            
        return dummy_node.next
```

## [86. Partition List](https://leetcode.com/problems/partition-list/)

### **Description**

Given the `head` of a linked list and a value `x`, partition it such that all nodes less than `x` come before nodes greater than or equal to `x`. You should preserve the original relative order of the nodes in each partition.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,4,3,2,5,2], x = 3`
- **Output**: `[1,2,2,4,3,5]`

---

**Example 2:**

- **Input**: `head = [2,1], x = 2`
- **Output**: `[1,2]`

---

</details>

### **Solution**

**Idea:** 根据 `x` 的值，将链表分为两部分，一部分是小于 `x` 的链表 `less`，另一部分是大于等于 `x` 的链表 `more`。最后将两部分链表拼接起来即可。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def partition(self, head: Optional[ListNode], x: int) -> Optional[ListNode]:
        less_head = less =  ListNode(0)
        more_head = more =  ListNode(0)
        cur = head
        while cur:
            if cur.val < x:
                less.next = cur
                less = less.next
            else:
                more.next = cur
                more = more.next
            cur = cur.next
        more.next = None
        less.next = more_head.next
        return less_head.next
```

## [146. LRU Cache](https://leetcode.com/problems/lru-cache/)

### **Description**

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with positive size `capacity`.
- `int get(int key)` Return the value of the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, evict the least recently used key.

The functions `get` and `put` must each run in `O(1)` time complexity.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]`
- **Output**: `[null, null, null, 1, null, -1, null, -1, 3, 4]`

---

</details>

### **Solution**

**Idea:** 使用双向链表和哈希表来实现 `LRU` 缓存。**双向链表** 用于维护节点的顺序，**哈希表** 用于存储节点和快速查找节点。

使用 **双向链表** 而非 **单向链表** 的原因是：在删除节点时，单向链表需要遍历链表找到前一个节点，时间复杂度为 _O(n)_ 。而双向链表可以直接找到前一个节点，时间复杂度为 _O(1)_ 。

`get` 操作：

1. 如果 `key` 存在，则将该节点移到链表尾部，并返回该节点的值。
2. 如果 `key` 不存在，则返回 `-1`。

`put` 操作：

1. 如果 `key` 存在，则更新该节点的值，并将其移到链表尾部。
2. 如果 `key` 不存在，则插入该节点。如果插入后链表长度超过 `capacity`，则删除链表头部的节点。

**Complexity:** Time: _O(1)_, Space: _O(capacity)_

```python
class ListNode:
    def __init__(self, key: int, value: int):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

class LRUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.head = ListNode(0, 0)
        self.tail = ListNode(0, 0)
        self.head.next = self.tail
        self.tail.prev = self.head

    def remove(self, node: ListNode):
        """Remove a node from the linked list."""
        prev_node = node.prev
        next_node = node.next
        prev_node.next = next_node
        next_node.prev = prev_node

    def add(self, node: ListNode):
        """Add a node right before the tail."""
        prev_tail = self.tail.prev
        prev_tail.next = node
        node.prev = prev_tail
        node.next = self.tail
        self.tail.prev = node

    def get(self, key: int) -> int:
        if key in self.cache:
            node = self.cache[key]
            self.remove(node)
            self.add(node)
            return node.value
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.remove(self.cache[key])
        elif len(self.cache) >= self.capacity:
            lru_node = self.head.next
            self.remove(lru_node)
            del self.cache[lru_node.key]
        new_node = ListNode(key, value)
        self.cache[key] = new_node
        self.add(new_node)
```