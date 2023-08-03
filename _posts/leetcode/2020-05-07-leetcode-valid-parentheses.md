---
layout: post
title: Leetcode刷题 - 20.有效的括号
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 20.有效的括号

难度：简单

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：<br>
左括号必须用相同类型的右括号闭合。<br>
左括号必须以正确的顺序闭合。<br>
注意空字符串可被认为是有效字符串。<br>
<!--more-->

示例 1:

   - 输入: "()"
   - 输出: true

示例 2:

   - 输入: "()[]{}"
   - 输出: true

示例 3:

   - 输入: "(]"
   - 输出: false

示例 4:

   - 输入: "([)]"
   - 输出: false

示例 5:

   - 输入: "{[]}"
   - 输出: true

```python
class Solution(object):
    """栈
        复杂度分析：
            时间复杂度：O(n)，因为我们一次只遍历给定的字符串中的一个字符并在栈上进行 O(1) 的推入和弹出操作。
            空间复杂度：O(n)，当我们将所有的开括号都推到栈上时以及在最糟糕的情况下，我们最终要把所有括号推到栈上。例如 ((((((((((。
    """
    def is_valid(self, s):
        # The stack to keep track of opening brackets.
        stack = []

        # Hash map for keeping track of mappings. This keeps the code very clean.
        # Also makes adding more types of parenthesis easier
        mapping = {")": "(", "}": "{", "]": "["}

        # For every bracket in the expression.
        for char in s:
            # If the character is an closing bracket
            if char in mapping:
                # Pop the topmost element from the stack, if it is non empty
                # Otherwise assign a dummy value of '#' to the top_element variable
                top_element = stack.pop() if stack else '#'

                # The mapping for the opening bracket in our hash and the top
                # element of the stack don't match, return False
                if mapping[char] != top_element:
                    return False
            else:
                # We have an opening bracket, simply push it onto the stack.
                stack.append(char)

        # In the end, if the stack is empty, then we have a valid expression.
        # The stack won't be empty for cases like ((()
        return not stack
```

运行测试用例：

```python
testcases = [
    ("()", True),
    ("(]", False),
    ("{[]}", True),
    ("([)]", False),
    ("()[]{}", True)
]
s = Solution()
for tc, val in testcases:
    assert(s.is_valid(tc) == val)
    print(val)

# Output:
# True
# False
# True
# False
# True
```
