---
title: Leetcode Binary Tree General Part 3
date: 2025-01-16 22:43:57
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [173. Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

### **Description**

Implement the `BSTIterator` class that represents an iterator over the in-order traversal of a binary search tree (BST):

- `BSTIterator(root)` Initializes an object of the class. The root of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `next()` Returns the next smallest number.
- `hasNext()` Returns `true` if there exists a number in the iterator or `false` otherwise.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]`, `[root = [7, 3, 15, null, null, 9, 20]]`
- **Output**: `[null, 3, 7, true, 9, true, 15, true, 20, false]`

---

**Example 2:**

- **Input**: `["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]`, `[root = [3, 1, 4, null, 2]]`
- **Output**: `[null, 1, 2, true, 3, true, 4, false]`

---

</details>

### **Solution**

**Idea:** 使用栈来模拟中序遍历的过程。总体思路是 **栈中只保留左节点**。

初始化时，将根节点及其所有左子树的左节点压入栈中。每次 `next()` 时，从栈中弹出当前节点，并将当前节点的右节点及其所有左子树的左节点压入栈中。

![中序遍历](/img/leetcode173.png)

**Complexity:** Time: _O(1)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class BSTIterator:

    def __init__(self, root: Optional[TreeNode]):
        self.stack = []
        while root:
            self.stack.append(root)
            root = root.left

    def next(self) -> int:
        cur = self.stack.pop()
        right_node = cur.right
        while right_node:
            self.stack.append(right_node)
            right_node = right_node.left
        return cur.val

    def hasNext(self) -> bool:
        return len(self.stack) > 0 
```

## [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

### **Description**

Given the `root` of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2^h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,3,4,5,6]`
- **Output**: `6`

---

**Example 2:**

- **Input**: `root = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]`
- **Output**: `15`

---

</details>

### **Solution**

**Idea:** 使用递归，完全二叉树的节点数等于左子树的节点数加上右子树的节点数再加上根节点。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        return 1 + self.countNodes(root.left) + self.countNodes(root.right) 
```

## [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

### **Description**

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1`
- **Output**: `3`

---

**Example 2:**

- **Input**: `root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4`
- **Output**: `5`

---

</details>

### **Solution**

**Idea:** 使用递归。

递归的终止条件是：

- 当前节点为空，返回 `False`
- 当前节点为 `p` 或 `q`，返回当前节点

递归过程：

- 递归左子树，找到左子树中是否存在 `p` 或 `q`
- 递归右子树，找到右子树中是否存在 `p` 或 `q`
- 如果左子树和右子树中分别存在 `p` 或 `q`，则当前节点为它们的公共祖先
- 如果左子树中存在 `p` 或 `q`，则返回左子树中的结果
- 如果右子树中存在 `p` 或 `q`，则返回右子树中的结果

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return False
        if root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root
        if left:
            return left
        if right:
            return right
```

## 二叉树问题总结

### 常见解题思路

1. **递归（DFS）**
   - 最常用的解决二叉树问题的方法
   - 适用于需要遍历整棵树的场景
   - 主要遍历方式：
     - 前序遍历（根-左-右）
     - 中序遍历（左-根-右）
     - 后序遍历（左-右-根）

2. **迭代（BFS）**
   - 使用队列实现层序遍历
   - 适用于需要按层处理节点的场景
   - 常用数据结构：`deque`（双端队列），可以实现时间复杂度为 _O(1)_ 的插入和删除操作

3. **分治法**
   - 将问题分解为左右子树的子问题
   - 合并左右子树的结果

### 如何写出递归代码

1. **确定递归函数的参数和返回值**
   ```python
   def recursion(root: TreeNode) -> Type:
       # 函数参数要包含解决问题所需的所有信息
       # 返回值要能够传递需要的信息给上层调用
   ```

2. **确定终止条件**
   ```python
   def recursion(root: TreeNode) -> Type:
       if not root:  # 最常见的终止条件
           return specific_value
       if other_condition:  # 其他可能的终止条件
           return other_value
   ```

3. **确定单层递归逻辑**
   ```python
   def recursion(root: TreeNode) -> Type:
       if not root:
           return specific_value
           
       # 处理当前节点
       result = process_current_node(root)
       
       # 递归处理左右子树
       left_result = recursion(root.left)
       right_result = recursion(root.right)
       
       # 整合结果
       return combine_results(result, left_result, right_result)
   ```

### 解题技巧

1. **使用哈希表优化查找**
   - 在树的构造问题中常用
   - 可以将 O(n) 的查找优化为 O(1)

2. **使用全局变量**
   - 在需要在递归过程中维护状态时使用
   - 注意变量的重置时机

3. **自顶向下 vs 自底向上**
   - 自顶向下：通过参数传递信息
   - 自底向上：通过返回值传递信息

通过掌握这些模式和技巧，我们可以更系统地解决二叉树相关的问题。关键是要认识到大多数二叉树问题都可以通过递归来解决，而写好递归的关键在于：
1. 明确递归函数的定义和功能
2. 设置正确的终止条件
3. 处理好当前层级的逻辑
4. 正确地组合子问题的结果

