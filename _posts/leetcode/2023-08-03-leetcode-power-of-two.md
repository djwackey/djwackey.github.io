---
layout: post
title: Leetcode刷题 - 231.2的幂
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 231.2的幂

难度：简单

给定一个整数，编写一个函数来判断它是否是 2 的幂次方。
<!--more-->

示例 1:

   - 输入: 1
   - 输出: true
   - 解释: $$ 2^{0} $$ = 1

示例 2:

   - 输入: 16
   - 输出: true
   - 解释: $$ 2^{4} $$ = 16

示例 3:

   - 输入: 218
   - 输出: false

```python
class Solution:
    def is_power_of_two(self, n):
        if n == 0:
            return False
        while n % 2 == 0:
            n /= 2
        return n == 1


class Solution1:
    """方法一：位运算：获取二进制中最右边的 1
    """
    def is_power_of_two(self, n):
        return False if n == 0 else n & (-n) == n


class Solution2:
    """方法二：位运算：去除二进制中最右边的 1
    """
    def is_power_of_two(self, n):
        return False if n == 0 else n & (n - 1) == 0
```

运行测试用例：

```python
def test_power_of_two():
    testcases = [(1, True), (16, True), (218, False), (0, False)]

    s0 = Solution()
    s1 = Solution1()
    s2 = Solution2()

    for num, out in testcases:
        assert s0.is_power_of_two(num) == out
        assert s1.is_power_of_two(num) == out
        assert s2.is_power_of_two(num) == out
```
