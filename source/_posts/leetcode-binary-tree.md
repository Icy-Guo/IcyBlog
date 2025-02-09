---
title: Leetcode Binary Tree
date: 2024-01-17 22:41:47
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### **Description**

Given the `root` of a binary tree, return the inorder traversal of its nodes' values.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,null,2,3]`
- **Output**: `[1,3,2]`

---

**Example 2:**

- **Input**: `root = []`
- **Output**: `[]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，分别计算左子树和右子树的遍历结果，然后合并。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
```

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

## [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

### **Description**

Given the `root` of a binary tree, return the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in the tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,3,4,5]`
- **Output**: `3`

---

**Example 2:**

- **Input**: `root = [1,2]`
- **Output**: `1`

---

</details>

### **Solution**

**Idea:** 树的 **直径** 等于是某个节点的 **左子树最大深度 + 右子树最大深度** 之和。使用深度优先搜索的方法，计算每个节点的左右子树的最大深度，然后取两者中的最大值加 1 作为当前节点的最大深度返回。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.max_diameter = 0
        
        def dfs(root):
            if not root:
                return 0
            
            left_depth = dfs(root.left)
            right_depth = dfs(root.right)

            self.max_diameter = max(self.max_diameter, left_depth + right_depth)
            return max(left_depth, right_depth) + 1

        dfs(root)
        return self.max_diameter
```

## [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### **Description**

Given the `root` of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [3,9,20,null,null,15,7]`
- **Output**: `[[3],[9,20],[15,7]]`

---

**Example 2:**

- **Input**: `root = [1]`
- **Output**: `[[1]]`

---

</details>

### **Solution**

**Idea:** 使用 `BFS` 从左到右遍历二叉树，每次将当前一层的所有节点的值加入结果列表中。用 `level` 来记录当前层的节点值。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        queue = deque([root])
        ans = []

        while queue:
            n = len(queue)
            level = []
            for i in range(n):
                node = queue.popleft()
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            ans.append(level)
        
        return ans
```

## [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### **Description**

Given an integer array `nums` where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `nums = [-10,-3,0,5,9]`
- **Output**: `[0,-3,9,-10,null,5]`

---

**Example 2:**

- **Input**: `nums = [1,3]`
- **Output**: `[3,1]`

---

</details>

### **Solution**

**Idea:** 使用递归的方法，将数组分成两部分，分别作为左右子树，然后递归地构建左右子树。

**Complexity:** Time: _O(n)_, Space: _O(log n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def build(left, right):
            if left > right:
                return None
            
            mid = (left + right) // 2
            root = TreeNode(nums[mid])
            root.left = build(left, mid - 1)
            root.right = build(mid + 1, right)
            return root
        
        return build(0, len(nums) - 1)
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

## [199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

### **Description**

Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return the values of the nodes you can see ordered from top to bottom.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [1,2,3,null,5,null,4]`
- **Output**: `[1,3,4]`

---

**Example 2:**

- **Input**: `root = [1,null,3]`
- **Output**: `[1,3]`

--- 

</details>

### **Solution**

**Idea:** 使用 `BFS` 从右到左遍历二叉树，每次将当前层的最后一个节点加入结果列表中。具体实现时，使用一个队列来存储当前层的节点，并使用一个变量来记录当前层的节点数。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
            
        queue = deque([root])
        ans = []
        
        while queue:
            n = len(queue)
            for i in range (n):
                node = queue.popleft()
                if i == n - 1:
                    ans.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return ans
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

## [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

### **Description**

Given the `root` of a binary tree and an integer `targetSum`, return the number of paths where the sum of the values along the path equals `targetSum`.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8`
- **Output**: `3`

---

**Example 2:**

- **Input**: `root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22`
- **Output**: `3`

---

</details>

### **Solution**

**Idea:** 使用前缀和，遍历二叉树，计算当前路径的和。如果当前路径的和减去 `targetSum` 在哈希表中存在，则说明存在一条路径的和为 `targetSum`，计数器加一。注意在回溯时，需要将当前路径的和减去一，否则会重复计算。

**Complexity:** Time: _O(n)_, Space: _O(h)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        pre_sum = {0: 1}

        def dfs(root, cur_sum):
            if not root:
                return 0

            cur_sum += root.val
            count = pre_sum.get(cur_sum - targetSum, 0)
            pre_sum[cur_sum] = pre_sum.get(cur_sum, 0) + 1

            count += dfs(root.left, cur_sum)
            count += dfs(root.right, cur_sum)

            pre_sum[cur_sum] -= 1 #回溯

            return count
        
        return dfs(root, 0)
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

---

`Top Interview 150 补充`

---

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

## [637. Average of Levels in Binary Tree](https://leetcode.com/problems/average-of-levels-in-binary-tree/)

### **Description**

Given the `root` of a binary tree, return the average value of the nodes on each level.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**
- **Input**: `root = [3,9,20,null,null,15,7]`
- **Output**: `[3.00000,14.50000,11.00000]`

---

**Example 2:**

- **Input**: `root = [3,9,20,15,7]`
- **Output**: `[3.00000,14.50000,11.00000]`

---

</details>

### **Solution**

**Idea:** 使用 `BFS` 从左到右遍历二叉树，每次将当前层的节点值累加，并计算平均值。具体实现时，使用一个队列来存储当前层的节点，并使用一个变量来记录这一层所有节点的值的总和。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        ans = []
        queue = deque([root])

        while queue:
            n = len(queue)
            total = 0

            for i in range(n):
                node = queue.popleft()
                total += node.val

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            avg = total / n
            ans.append(avg)

        return ans
```

## [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

### **Description**

Given the `root` of a binary tree, return the zigzag level order traversal of its nodes' values. (i.e., from left to right, then right to left for the next level and alternate between).

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `root = [3,9,20,null,null,15,7]`
- **Output**: `[[3],[20,9],[15,7]]`

---

**Example 2:**

- **Input**: `root = [1]`
- **Output**: `[[1]]`

---

</details>

### **Solution**

**Idea:** 使用 `BFS` 从左到右遍历二叉树，每次将当前一层的所有节点的值加入结果列表中。用 `left_to_right` 来记录当前层是从左到右还是从右到左。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        queue = deque([root])
        ans = []
        left_to_right = False

        while queue:
            n = len(queue)
            level = []
            for i in range(n):
                node = queue.popleft()
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            left_to_right = not left_to_right
            if not left_to_right:
                level.reverse()
            ans.append(level)
        
        return ans
```

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

