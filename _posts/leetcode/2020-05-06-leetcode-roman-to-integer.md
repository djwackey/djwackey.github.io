---
layout: post
title: Leetcode刷题 - 罗马数字转整数
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 13.罗马数字转整数

难度：简单

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

| 字符 | 数值 |
| -    | -    |
| I    | 1    |
| V    | 5    |
| X    | 10   |
| L    | 50   |
| C    | 100  |
| D    | 500  |
| M    | 1000 |

例如，罗马数字2写做II，即为两个并列的1。12写做XII，即为X+II。27写做XXVII, 即为XX+V+II。<br>

通常情况下，罗马数字中小的数字在大的数字的右边。<br>
但也存在特例，例如4不写做IIII，而是IV。数字1在数字5的左边，所表示的数等于大数5减小数1得到的数值4。
<!--more-->
同样地，数字9表示为IX。这个特殊的规则只适用于以下六种情况：

   - I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
   - X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。
   - C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

示例 1:

   - 输入: "III"
   - 输出: 3

示例 2:

   - 输入: "IV"
   - 输出: 4

示例 3:

   - 输入: "IX"
   - 输出: 9

示例 4:

   - 输入: "LVIII"
   - 输出: 58
   - 解释: L = 50, V= 5, III = 3.

示例 5:

   - 输入: "MCMXCIV"
   - 输出: 1994
   - 解释: M = 1000, CM = 900, XC = 90, IV = 4.

```python
class Solution(object):
    def roman_to_integer(self, s):
        total, prev = 0, self.get_val(s[0])
        for c in s[1:]:
            value = self.get_val(c)
            if prev < value:
                total -= prev
            else:
                total += prev
            prev = value
        total += prev
        return total
    
    def get_val(self, c):
        d = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000
        }
        return d.get(c, 0)


testcases = [('D', 500), ("IV", 4), ("IX", 9), ("III", 3), ("LVIII", 58), ("MCMXCIV", 1994)]
s = Solution()
for tc, val in testcases:
    assert(s.roman_to_integer(tc) == val)
    print('The roman \"{}\" is {}.'.format(tc, val))

# Output:
# The roman "D" is 500.
# The roman "IV" is 4.
# The roman "IX" is 9.
# The roman "III" is 3.
# The roman "LVIII" is 58.
# The roman "MCMXCIV" is 1994.
```
