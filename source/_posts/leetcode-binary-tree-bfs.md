---
title: Leetcode Binary Tree BFS
date: 2025-01-19 14:40:29
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

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