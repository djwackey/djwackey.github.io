---
layout: post
title: Leetcode刷题 - 整数反转
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 7.整数反转

难度：简单

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

示例1:

   - 输入：123
   - 输出：321

示例2：

   - 输入：-123
   - 输出：-321

示例3：

   - 输入：120
   - 输出：21

注意：

   - 假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [$$ −2^{31} $$, $$ 2^{31−1} $$]。
   - 请根据这个假设，如果反转后整数溢出那么就返回 0。
<!--more-->

```python
if 'INT_MAX' not in locals():
    INT_MAX = 2**31 - 1
if 'INT_MIN' not in locals():
    INT_MIN = -2**31


class Solution1(object):
    def reverse(self, v):
        assert(isinstance(v, int))
        sv = str(v)[::-1]
        if '-' == sv[-1]:
            sv = '-{}'.format(sv[:-1])
            return int(sv) if int(sv) >= INT_MIN else 0
        return int(sv) if int(sv) <= INT_MAX else 0


testcases = [(123, 321), (-123, -321), (120, 21)]
s = Solution1()
for tc, val in testcases:
    assert(s.reverse(tc) == val)
    print(val)

# Output:
# 321
# -321
# 21


class Solution2(object):
    """弹出和推入数字 & 溢出前进行检查
        复杂度分析：
            时间复杂度：O(log(x))，x中大约有log(x)位数字。
            空间复杂度：O(1)。
    """
    def reverse(self, v):
        assert(isinstance(v, int))
        recv, x = 0, abs(v)
        while x != 0:
            pop = x % 10
            x = x // 10
            if recv > INT_MAX // 10 or (recv == INT_MAX // 10 and pop > INT_MAX % 10):
                recv = 0
                break
            elif recv < INT_MIN // 10 or (recv == INT_MIN // 10 and pop < -(abs(INT_MIN) % 10)):
                recv = 0
                break
            recv = recv * 10 + pop
        return recv if v > 0 else -recv


print(INT_MIN, INT_MAX, -(abs(INT_MIN) % 10), INT_MAX % 10)
# Output:
# -2147483648 2147483647 -8 7


testcases = [(123, 321), (-123, -321), (120, 21), (8463847412, 0)]
s = Solution2()
for tc, val in testcases:
    assert(s.reverse(tc) == val)
    print(val)

# Output:
# 321
# -321
# 21
# 0
```
