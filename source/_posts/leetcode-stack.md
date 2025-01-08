---
title: Leetcode Stack
date: 2025-01-08 09:25:15
tags:
  - Study
  - Leetcode
categories:
  - Study
top_img: /img/leetcode.jpeg
cover: /img/leetcode.jpeg
---

## [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### **Description**

Given a string `s` containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

An input string is valid if:

1. Open brackets are closed by the same type of brackets.
2. Open brackets are closed in the correct order.
3. Every close bracket has a corresponding open bracket.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `s = "()"`
- **Output**: `true`

---

**Example 2:**

- **Input**: `s = "()[]{}"`
- **Output**: `true`

---

</details>

### **Solution**

**Idea:** 使用栈来存储括号，如果遇到闭合括号，则检查栈顶是否为对应的开放括号。如果是，则弹出栈顶；否则，返回 `False`。最后，如果栈为空，则返回 `True`，否则返回 `False`。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution:
    def isValid(self, s: str) -> bool:
        pairs = {')': '(', '}': '{', ']': '['}
        stack = []
        for c in s:
            if c in pairs:
                if not stack or stack[-1] != pairs[c]:
                    return False
                else:
                    stack.pop()
            else:
                stack.append(c)
        return not stack
```

## [71. Simplify Path](https://leetcode.com/problems/simplify-path/)

### **Description**

Given a string `path`, which is an absolute path (starting with a slash `/`) to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a Unix-style file system, a period `.` refers to the current directory, a double period `..` refers to the directory up a level, and any multiple consecutive slashes (e.g., `///`) are treated as a single slash `/`. For this problem, any other format of periods such as `...` or `..a` are treated as file/directory names.

The canonical path should have the following format:

- The path starts with a single slash `/`.
- Any two directories are separated by a single slash `/`.
- The path does not end with a trailing `/`.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `.` or double period `..`).

Return the simplified canonical path.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `path = "/home/"`
- **Output**: `"/home"`

---

**Example 2:**

- **Input**: `path = "/../"`
- **Output**: `"/"`

---

</details>

### **Solution**

**Idea:** 使用栈来处理路径，遇到 `.` 或 `..` 时，根据情况进行处理。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        names = path.split("/")
        stack = []
        for name in names:
            if name == '..':
                if stack:
                    stack.pop()
            elif name == '.':
                continue
            elif name:
                stack.append(name)
        return "/" + "/".join(stack)
```

## [155. Min Stack](https://leetcode.com/problems/min-stack/)

### **Description**

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element val onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `["MinStack","push","push","push","getMin","pop","top","getMin"]`
- **Output**: `[null,null,null,null,0,null,0,0]`

---

</details>

### **Solution**

**Idea:** 题目要求在常数时间内完成所有操作。其中， `__init__`、`push`、`pop`、`top` 操作的时间复杂度本身就为 _O(1)_，而常规的 `getMin` 操作需要遍历整个栈，时间复杂度为 _O(n)_。为了满足常数时间复杂度的要求，需要使用两个栈，一个栈用于存储数据本身，另一个栈用于存储最小值。 `push` 操作时，将最小值压入第二个栈，`pop` 操作时，将最小值弹出。这样，`getMin` 操作只需要返回第二个栈的栈顶元素即可，时间复杂度为 _O(1)_。

**Complexity:** Time: _O(1)_, Space: _O(n)_

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if not self.min_stack:
            self.min_stack.append(val)
        else:
            self.min_stack.append(min(val, self.min_stack[-1]))

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

## [150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

### **Description**

You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

- The valid operators are `+`, `-`, `*`, and `/`.
- Each operand may be an integer or another expression.
- The division between two integers always truncates toward zero.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a 32-bit integer.

<details>
<summary><b>Click to view full description</b></summary>

---

**Example 1:**

- **Input**: `tokens = ["2","1","+","3","*"]`
- **Output**: `9`

---

**Example 2:**

- **Input**: `tokens = ["4","13","5","/","+"]`
- **Output**: `6`

---

</details>

### **Solution**

**Idea:** 使用栈来处理逆波兰表达式，遇到操作符时，将栈顶的两个元素进行相应的操作，并将结果压入栈中。最后，栈中剩下的唯一元素即为结果。需要注意除法运算需要向零取整。

**Complexity:** Time: _O(n)_, Space: _O(n)_

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        def cal(x, y, op):
            if op == '+':
                return x + y
            elif op == '-':
                return x - y
            elif op == '*':
                return x * y
            elif op == '/':
                return int(x / y)
        
        stack = []
        for token in tokens:
            if token == '+' or token == '-' or token == '*' or token == '/':
                num2 = stack.pop()
                num1 = stack.pop()
                result = cal(num1, num2, token)
            else:
                result = int(token)
            stack.append(result)
        return stack[0]
```
