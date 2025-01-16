---
title: Leetcode Binary Tree General Part 1
date: 2025-01-14 10:46:22
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### **Description**

Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [3,9,20,null,null,15,7]`
- **Output**: `3`

---

**Example 2:**

- **Input**: `root = [1,null,2]`
- **Output**: `2`

---

</details>

### **Solution**

#### **Approach 1:** 深度优先搜索

**Idea:** 使用递归的方法，分别计算左子树和右子树的最大深度，然后取两者中的最大值加 1 作为当前节点的最大深度。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root == None:
            return 0
        else:
            left_height = self.maxDepth(root.left)
            right_height = self.maxDepth(root.right)
            return max(left_height, right_height) + 1
```

#### **Approach 2:** 广度优先搜索

**Idea:** 使用广度优先搜索的方法，遍历每一层节点，并记录深度。

Python 中使用 `deque` 来实现广度优先搜索。`deque` 是双端队列，可以高效地从两端进行插入和删除操作，时间复杂度为 _O(1)_。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        queue = deque([root])
        depth = 0
        while queue:
            length = len(queue)
            for _ in range(length):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            depth += 1
        return depth       
```

## [100. Same Tree](https://leetcode.com/problems/same-tree/)

### **Description**

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `p = [1,2,3], q = [1,2,3]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `p = [1,2], q = [1,null,2]`
- **Output**: `false`

---

</details>

### **Solution**

#### **Approach 1:** 深度优先搜索

**Idea:** 使用递归的方法，分别比较两个树的根节点、左子树和右子树。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        elif not p or not q:
            return False
        elif p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

#### **Approach 2:** 广度优先搜索

**Idea:** 用两个队列分别存储两个树的节点，然后依次比较两个队列中的节点。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        elif not p or not q:
            return False
        
        q1 = deque([p])
        q2 = deque([q])

        while q1 and q2:
            node1 = q1.popleft()
            node2 = q2.popleft()
            if node1.val != node2.val:
                return False
            left1, right1 = node1.left, node1.right
            left2, right2 = node2.left, node2.right
            if (not left1 and left2) or (not left2 and left1) or (not right1 and right2) or (not right2 and right1):
                return False
            if left1:
                q1.append(left1)
            if right1:
                q1.append(right1)
            if left2:
                q2.append(left2)
            if right2:
                q2.append(right2)

        return not q1 and not q2
```

## [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

### **Description**

Given the `root` of a binary tree, invert the tree, and return its root.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [4,2,7,1,3,6,9]`
- **Output**: `[4,7,2,9,6,3,1]`

---

**Example 2:**

- **Input**: `root = [2,1,3]`
- **Output**: `[2,3,1]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，分别交换每个节点的左右子树。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root
        
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        root.left = right
        root.right = left

        return root
```

## [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

### **Description**

Given the `root` of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,2,3,4,4,3]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `root = [1,2,2,null,3,null,3]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，分别比较两个子树是否对称, 递归地比较 **左子树的左节点** 和 **右子树的右节点** 以及 **左子树的右节点** 和 **右子树的左节点**。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def check(left, right):
            if not left and not right:
                return True
            elif not left or not right:
                return False
            return left.val == right.val and check(left.left, right.right) and check(left.right, right.left)

        return check(root.left, root.right)
```

## [105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### **Description**

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return the binary tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]`
- **Output**: `[3,9,20,null,null,15,7]`

---

**Example 2:**

- **Input**: `preorder = [-1], inorder = [-1]`
- **Output**: `[-1]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，分别构建左子树和右子树。每次递归时，从 `preorder` 中取出第一个元素作为根节点，然后在中序遍历中找到根节点的位置 `in_root`，将中序遍历分为左右两部分，分别构建左子树和右子树。需要使用一个哈希表来存储中序遍历中每个元素的位置，以便快速找到根节点的位置（时间复杂度为 _O(1)_），这样可以使总时间复杂度从 _O(n ^ 2)_ 降低到为 _O(n)_。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        def myBuildTree(in_left, in_right):
            if in_left > in_right:
                return None
            
            # 获取前序遍历中的第一个元素作为根节点
            root_val = preorder.pop(0)  # 从preorder获取根节点值
            root = TreeNode(root_val)
            
            # 在中序遍历中找到根节点位置
            in_root = index[root_val]

            # 构建左子树
            root.left = myBuildTree(in_left, in_root - 1)
            # 构建右子树
            root.right = myBuildTree(in_root + 1, in_right)
            
            return root

        n = len(preorder)
        index = {num : i for i, num in enumerate(inorder)}
        return myBuildTree(0, n-1)
```
