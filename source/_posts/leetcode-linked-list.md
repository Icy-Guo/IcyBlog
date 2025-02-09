---
title: Leetcode Linked List
date: 2025-01-04 22:41:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### **Description**

Given the heads of two singly linked-lists `headA` and `headB`, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return `null`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3`
- **Output**: `Intersected at '8'`

---

**Example 2:**

- **Input**: `intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1`
- **Output**: `Intersected at '2'`

---

</details>

### **Solution**

**Idea:** 使用双指针，两个指针分别遍历两个链表。指针遍历完一个链表后，再遍历另一个链表，这样可以保证两个链表会 **同时到达交点** 或者 **同时到达链表末尾（如果没有交点）**。

**Complexity:** Time: _O(m + n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if not headA or not headB:
            return None

        a = headA
        b = headB

        while a != b:
            a = a.next if a else headB
            b = b.next if b else headA

        return a
```

## [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### **Description**

Given the `head` of a singly linked list, reverse the list, and return the reversed list.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,4,5]`
- **Output**: `[5,4,3,2,1]`

---

**Example 2:**

- **Input**: `head = [1,2]`
- **Output**: `[2,1]`

---

</details>

### **Solution**

**Idea:** 使用迭代法，遍历链表，将每个节点的 `next` 指向前一个节点，最后返回 `pre` 即为最后一个节点。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        cur = head

        while cur:
            next_node = cur.next
            cur.next = pre
            pre = cur
            cur = next_node
        
        return pre
```

## [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

### **Description**

Given the `head` of a singly linked list, return `true` if it is a palindrome or `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,2,1]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `head = [1,2]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用快慢指针找到链表的中点，然后反转后半部分链表（`slow` 指针之后的部分），最后比较前半部分和后半部分是否一致。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:

        # 1. 使用快慢指针找到链表的中点
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

         # 2. 反转后半部分链表
        prev = None
        while slow:
            next_node = slow.next
            slow.next = prev
            prev = slow
            slow = next_node

        # 3. 比较前半部分和后半部分
        left, right = head, prev
        while right:
            if right.val != left.val:
                return False
            right = right.next
            left = left.next
        return True
```

## [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

### **Description**

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [3,2,0,-4], pos = 1`
- **Output**: `true`

---

**Example 2:**

- **Input**: `head = [1,2], pos = 0`
- **Output**: `true`

---

</details>

### **Solution**

**Idea:** 使用快慢指针，如果快指针和慢指针相遇，则说明有环。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head

        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next

            if fast == slow:
                return True
        
        return False
```

## [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

### **Description**

Given the `head` of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [3,2,0,-4], pos = 1`
- **Output**: `2`

---

**Example 2:**

- **Input**: `head = [1,2], pos = 0`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用快慢指针，如果快慢指针相遇，则说明有环。然后从相遇点开始，将 `slow` 指针移动到链表头部，然后将 `slow` 和 `fast` 指针每次移动一步，直到相遇，相遇点即为环的入口。

**为什么 `slow` 指针从链表头部开始，会和 `fast` 指针在环的入口相遇？**

- `x`：从 head 到环的起始点的距离
- `y`：环的起始点到 slow 和 fast 第一次相遇点的距离
- `z`：从相遇点回到环的起始点的距离（即环的剩余部分）

假设 `slow` 和 `fast` 在相遇时，`slow` 走了 `x + y` 步，`fast` 走了 `x + y + z + y` 步。

因为 `fast` 的速度是 `slow` 的两倍，所以 `x + y + z + y = 2(x + y)`，即 `z = x`。

所以，`slow` 从链表头部开始，走 `x` 步，就会到达环的入口。而 `fast` 从相遇点开始，走 `z` 步，也会到达环的入口。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        
        return None
```

## [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

### **Description**

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `list1 = [1,2,4], list2 = [1,3,4]`
- **Output**: `[1,1,2,3,4,4]`

---

**Example 2:**

- **Input**: `list1 = [], list2 = [0]`
- **Output**: `[0]`

---

</details>

### **Solution**

**Idea:** 使用一个虚拟头节点来存储结果，然后遍历两个链表, 比较两个链表的值，将较小的值插入到虚拟头节点中，并移动指针。最后，将剩余的链表插入到虚拟头节点中。

**Complexity:** Time: _O(m + n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode(-1)
        current = dummy_head

        while list1 and list2:
            if list1.val <= list2.val:
                current.next = list1
                list1 = list1.next
            else:
                current.next = list2
                list2 = list2.next
            current = current.next

        if list1:
            current.next = list1
        else:
            current.next = list2

        return dummy_head.next
```

## [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

### **Description**

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `l1 = [2,4,3], l2 = [5,6,4]`
- **Output**: `[7,0,8]`

---

</details>

### **Solution**

**Idea:** 使用一个虚拟头节点来存储结果，然后遍历两个链表，计算每一位的和还有进位，并存储到虚拟头节点中。

**Complexity:** Time: _O(max(m, n))_, Space: _O(max(m, n))_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        dummy_head = ListNode()
        current = dummy_head
        carry = 0
        while l1 or l2 or carry:
            val1 = l1.val if l1 else 0
            val2 = l2.val if l2 else 0
            total = val1 + val2 + carry
            carry = total // 10
            current.next = ListNode(val = total % 10)
            current = current.next
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
        return dummy_head.next
```

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

## [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

### **Description**

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,4]`
- **Output**: `[2,1,4,3]`

---

**Example 2:**

- **Input**: `head = [1]`
- **Output**: `[1]`

---

</details>

### **Solution**

**Idea:** 使用一个虚拟头节点来存储结果，然后遍历链表，交换每两个相邻的节点。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        pre = dummy

        while head and head.next:
            first = head
            second = head.next
            
            pre.next = second
            first.next = second.next
            second.next = first

            pre = first
            head = first.next
        
        return dummy.next
```

## [138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

### **Description**

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a deep copy of the list. The deep copy should consist of exactly `n` brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointers of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return the head of the copied linked list.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the random pointer points to, or `null` if it does not point to any node.

Your code will only be given the head of the original linked list.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head=[[7,null],[13,0],[11,4],[10,2],[1,0]]`
- **Output**: `[[7,null],[13,0],[11,4],[10,2],[1,0]]`

---

**Example 2:**

- **Input**: `head = [[1,1],[2,1]]`
- **Output**: `[[1,1],[2,1]]`

---

</details>

### **Solution**

**Idea:** 第一步：复制每个节点，并插入到原节点之后（`1->1'->2->2'->3->3'`）。第二步：设置新节点的 `random` 指针。第三步：将复制链表和原链表拆分出来。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        if head == None:
            return None

        # 第一步：复制每个节点，并插入到原节点之后（`1->1'->2->2'->3->3'`）
        current = head
        while current:
            new_node = ListNode(current.val)
            new_node.next = current.next
            current.next = new_node
            current = current.next.next

        # 第二步：设置新节点的 `random` 指针
        current = head
        while current:
            next_node = current.next
            if current.random == None:
                next_node.random = None
            else:
                next_node.random = current.random.next
            current = current.next.next
            
        # 第三步：将复制链表和原链表拆分出来
        current = head
        new_head = head.next
        while current:
            next_node = current.next
            current.next = current.next.next
            if next_node.next:
                next_node.next = next_node.next.next
            else:
                next_node.next = None
            current = current.next
      
        return new_head
```

## [148. Sort List](https://leetcode.com/problems/sort-list/)

### **Description**

Given the `head` of a linked list, return the list after sorting it in ascending order.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [4,2,1,3]`
- **Output**: `[1,2,3,4]`

---

**Example 2:**  

- **Input**: `head = [-1,5,3,4,0]`
- **Output**: `[-1,0,3,4,5]`

---

</details>

### **Solution**

#### **Approach 1** 归并排序（递归）

**Idea:** 使用归并排序来对链表进行排序。

**归并排序** 是分治法的典型应用，它将链表分成两个部分，然后递归地对两个部分进行排序，最后将两个有序的链表合并成一个有序的链表。

具体步骤：

1. 找到链表的中点，将链表分成两个部分。
2. 递归地对两个部分进行排序。
3. 合并两个有序的链表。

**Complexity:** Time: _O(n log n)_, Space: _O(log n)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        
        # 1. 找到中点
        slow, fast = head, head.next
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        # 2. 拆分链表
        mid = slow.next
        slow.next = None

        # 3. 递归排序左右部分
        left = self.sortList(head)
        right = self.sortList(mid)

        # 4. 合并两个有序链表
        return self.merge(left, right)

    def merge(self, l1, l2):
        # 输入：两个有序链表的头节点
        # 输出：合并后的有序链表的头节点
        dummy = ListNode(0)
        cur = dummy
        while l1 and l2:
            if l1.val < l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        
        cur.next = l1 if l1 else l2  # 连接剩余部分
        return dummy.next
```

#### **Approach 2** 归并排序（迭代）

**Idea:** 使用迭代的方式来实现归并排序。

**Complexity:** Time: _O(n log n)_, Space: _O(1)_

迭代的方法可以将空间复杂度缩短为 _O(1)_， 但代码逻辑会变得复杂不易理解。具体可见[官方题解]([text](https://leetcode.cn/problems/sort-list/solutions/492301/pai-xu-lian-biao-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked))。

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

---

`Top Interview 150 补充`

---

## [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

### **Description**

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return the reversed list.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `head = [1,2,3,4,5], left = 2, right = 4`
- **Output**: `[1,4,3,2,5]`

---

**Example 2:**

- **Input**: `head = [5], left = 1, right = 1`
- **Output**: `[5]`

---

</details>

### **Solution**

**Idea:** 使用一个虚拟头节点来存储结果，然后遍历链表，找到需要反转的节点区间，并进行反转。

具体反转步骤：

1. 把`current.next`指向`next.next`
2. 把`next.next`指向`pre.next`
3. 把`pre.next`指向`next`

![Reverse steps](/img/leetcode92.png)

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        dummy_head = ListNode(0, head)
        pre = dummy_head

        for _ in range(left - 1):
            pre = pre.next

        current= pre.next
        for _ in range(right - left):
            next = current.next
            current.next = next.next
            next.next = pre.next
            pre.next = next

        return dummy_head.next 
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
