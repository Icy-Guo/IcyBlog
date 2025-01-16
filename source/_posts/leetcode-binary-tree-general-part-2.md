---
title: Leetcode Binary Tree General Part 2
date: 2025-01-16 22:19:38
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

### **Description**

Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return the binary tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]`
- **Output**: `[3,9,20,null,null,15,7]`

---

**Example 2:**

- **Input**: `inorder = [-1], postorder = [-1]`
- **Output**: `[-1]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，分别构建左子树和右子树。每次递归时，从 `postorder` 中取出最后一个元素作为根节点，然后在中序遍历中找到根节点的位置 `in_root`，将中序遍历分为左右两部分，分别构建左子树和右子树。需要使用一个哈希表来存储中序遍历中每个元素的位置，以便快速找到根节点的位置（时间复杂度为 _O(1)_），这样可以使总时间复杂度从 _O(n ^ 2)_ 降低到为 _O(n)_。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        def myBuildTree(in_left, in_right):
            if in_left > in_right:
                return None

            val = postorder.pop()
            root = TreeNode(val)
            in_root = index[val]

            root.right = myBuildTree(in_root + 1, in_right)
            root.left = myBuildTree(in_left, in_root - 1)

            return root

        n = len(inorder)
        index = {num : i for  i, num in enumerate(inorder)}
        return myBuildTree(0, n - 1)
```

## [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

### **Description**

Given a binary tree, populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,3,4,5,null,7]`
- **Output**: `[1,#,2,3,#,4,5,7,#]`

---

**Example 2:**

- **Input**: `root = []`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 使用迭代的方法，通过一个 `dummy` 节点来连接每一层的节点。每次迭代时，将 `dummy` 节点的 `next` 指向当前节点的左子节点或右子节点，然后将 `dummy` 节点指向 `dummy.next`，直到遍历完一层的所有节点。

**Complexity:** Time: _O(n)_, Space: _O(1)_

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return None
        
        dummy = Node(0)
        cur = root
        while cur:
            tail = dummy
            dummy.next = None
            while cur:
                if cur.left:
                    tail.next = cur.left
                    tail = tail.next
                if cur.right:
                    tail.next = cur.right
                    tail = tail.next
                cur = cur.next
            cur = dummy.next
        return root
```

## [114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

### **Description**

Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,5,3,4,null,6]`
- **Output**: `[1,null,2,null,3,null,4,null,5,null,6]`

---

**Example 2:**

- **Input**: `root = []`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，通过 **前序遍历** 将二叉树转换为 **链表**。然后遍历链表，将每个节点的 `left` 指针设置为 `null`，将每个节点的 `right` 指针设置为下一个节点。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def flatten(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        preorder = []

        def buildPreorder(root: TreeNode):
            if root:
                preorder.append(root)
                buildPreorder(root.left)
                buildPreorder(root.right)
        
        buildPreorder(root)
        size = len(preorder)
        for i in range(1, size):
            prev, cur = preorder[i - 1], preorder[i]
            prev.left = None
            prev.right = cur
```

## [112. Path Sum](https://leetcode.com/problems/path-sum/)

### **Description**

Given the `root` of a binary tree and an integer `targetSum`, return `true` if the tree has a root-to-leaf path such that adding up all the values along the path equals `targetSum`.

A leaf is a node with no children.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22`
- **Output**: `true`

---

**Example 2:**

- **Input**: `root = [1,2,3], targetSum = 5`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，如果当前节点是叶子节点，则判断当前节点的值是否等于 `targetSum`。如果当前节点不是叶子节点，则递归判断左子树或右子树中是否存在一条路径，使得路径上所有节点的值之和等于 `targetSum - root.val`。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        if not root.left and not root.right:
            return targetSum == root.val
        return self.hasPathSum(root.left, targetSum - root.val) or self.hasPathSum(root.right, targetSum - root.val)
```

## [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

### **Description**

You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.

- For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return the total sum of all root-to-leaf numbers. Test cases are generated so that the answer will fit in a 32-bit integer.

A leaf node is a node with no children.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,3]`
- **Output**: `25`

---

**Example 2:**

- **Input**: `root = [4,9,0,5,1]`
- **Output**: `1026`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，每次将总和 `total` 乘以 10 再加上当前节点的值，如果当前节点是叶子节点，则返回总和 `total`，否则递归计算左子树和右子树的和。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(root: Optional[TreeNode], total):
            if not root:
                return 0
            total = total * 10 + root.val
            if not root.left and not root.right:
                return total
            else:
                return dfs(root.left, total) + dfs(root.right, total)
        return dfs(root, 0)  
```
