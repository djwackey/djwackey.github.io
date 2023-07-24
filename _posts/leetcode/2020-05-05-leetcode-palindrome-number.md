---
layout: post
title: Leetcode刷题 - 回文数
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 9.回文数

难度：简单

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

示例1:

   - 输入：121
   - 输出：true

示例2：

   - 输入：-121
   - 输出：false

示例3：

   - 输入：10
   - 输出：false

进阶：

你能不将整数转为字符串来解决这个问题吗？
<!--more-->

```python
class Solution:
    """反转一半数字
        复杂度分析：
            时间复杂度：O(log(n))。
            空间复杂度：O(1)。
    """
    def is_palindrome(self, x):
        if x < 0 or (x % 10 == 0 and x != 0):
            return False
        nreverted = 0;
        while x > nreverted:
            nreverted = nreverted * 10 + x % 10
            x = x // 10
        return x == nreverted or x == nreverted // 10


testcases = [(121, True), (-121, False), (10, False), (11, True)]
s = Solution()
for tc, val in testcases:
    assert(s.is_palindrome(tc) == val), 'testcase:{},failed'.format(tc)
    print(f"Input: {tc}\tIs palindrome: {val}")

# Output:
# Input: 121    Is palindrome: True
# Input: -121   Is palindrome: False
# Input: 10     Is palindrome: False
# Input: 11     Is palindrome: True
```
