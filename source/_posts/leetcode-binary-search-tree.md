---
title: Leetcode Binary Search Tree
date: 2025-01-20 09:36:09
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

### **Description**

Given the `root` of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [4,2,6,1,3]`
- **Output**: `1`

---

**Example 2:**

- **Input**: `root = [1,0,48,null,null,12,49]`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 使用中序遍历，记录前一个节点的值，计算当前节点与前一个节点的差值，更新最小差值。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        self.pre = -1
        self.min_dif = float('inf')

        def inorder(root: Optional[TreeNode]):
            if not root:
                return 0

            inorder(root.left)

            if self.pre != -1:
                self.min_dif = min(self.min_dif, abs(self.pre - root.val))
            self.pre = root.val

            inorder(root.right)
        
        inorder(root)
        return self.min_dif
```

## [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### **Description**

Given the `root` of a binary search tree, and an integer `k`, return the `kth` smallest value (1-indexed) of all the values of the nodes in the tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [3,1,4,null,2], k = 1`
- **Output**: `1`

---

**Example 2:**

- **Input**: `root = [5,3,6,2,4,null,null,1], k = 3`
- **Output**: `3`

---

</details>

### **Solution**

**Idea:** 使用中序遍历，记录当前遍历的节点数，当遍历到第 `k` 个节点时，返回该节点的值。因为二叉搜索树的性质，中序遍历的结果是升序的，所以第 `k` 个节点就是第 `k` 小的元素。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        self.k = k
        self.ans = None

        def inorder(root: Optional[TreeNode]):
            if not root:
                return
            
            inorder(root.left)

            self.k -= 1
            if (self.k == 0):
                self.ans = root.val
            
            inorder(root.right)
        
        inorder(root)
        return self.ans
```

## [98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

### **Description**

Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [2,1,3]`
- **Output**: `true`

---

**Example 2:**

- **Input**: `root = [5,1,4,null,null,3,6]`
- **Output**: `false`

---

</details>

### **Solution**

**Idea:** 二叉搜索树的性质是左子树中的所有值小于根节点的值，右子树中的所有值大于根节点的值。使用递归，记录当前节点的值范围，如果当前节点的值不在范围之内，则返回 `false`。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def helper(root: Optional[TreeNode], lower = float('-inf'), upper = float('inf')):
            if not root:
                return True
            if not (lower < root.val < upper):
                return False
            return helper(root.left, lower, root.val) and helper(root.right, root.val, upper)
        return helper(root)
```