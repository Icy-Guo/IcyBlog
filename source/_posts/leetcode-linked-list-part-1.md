---
title: Leetcode Linked List Part 1
date: 2025-01-10 10:18:15
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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

        current = head
        while current:
            new_node = ListNode(current.val)
            new_node.next = current.next
            current.next = new_node
            current = current.next.next

        current = head
        while current:
            next_node = current.next
            if current.random == None:
                next_node.random = None
            else:
                next_node.random = current.random.next
            current = current.next.next
            
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
