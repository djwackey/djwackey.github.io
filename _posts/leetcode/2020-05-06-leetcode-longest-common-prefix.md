---
layout: post
title: Leetcode刷题 - 最长公共前缀
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 14.最长公共前缀

难度：简单

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串“”。

示例 1:

   - 输入: ["flower", "flow", "flight"]
   - 输出: "fl"

示例 2:

   - 输入: ["dog", "racecar", "car"]
   - 输出: ""
   - 解释: 输入不存在公共前缀。

> 说明: 所有输入只包含小写字母 a-z 。

```python
def longest_common_prefix1(strs):
    """水平扫描
        复杂度分析：
            时间复杂度：O(S)，S 是所有字符串中字符数量的总和。
                最坏的情况下，n个字符串都是相同的。算法会将 S1 与其他字符串 [S2...Sn] 都做一次比较。
                这样就会进行 S 次字符比较，其中 S 是输入数据中所有字符数量。
            空间复杂度：O(1)
    """
    assert(isinstance(strs, list))
    if len(strs) == 0:
        return ''
    prefix = strs[0]
    for s in strs[1:]:
        while not s.startswith(prefix):
            prefix = prefix[0:len(prefix) - 1]
            if not prefix:
                return ''
    return prefix


def longest_common_prefix2(strs):
    """水平扫描
    """
    assert(isinstance(strs, list))
    slen = len(strs)
    if slen == 0:
        return ''
    for i, c in enumerate(strs[0]):
        for j in range(1, slen):
            if i == len(strs[j]) or strs[j][i] != c:
                return strs[0][0:i]
    return strs[0]


class Solution(object):
    def longest_common_prefix(self, strs):
        """分治法
        """
        assert(isinstance(strs, list))
        slen = len(strs)
        if slen == 0:
            return ''
        return self._longest_common_prefix(strs, 0, slen - 1)
    
    def _longest_common_prefix(self, strs, l, r):
        if l == r:
            return strs[l]
        mid = (l + r) // 2
        left = self._longest_common_prefix(strs, l, mid)
        right = self._longest_common_prefix(strs, mid + 1, r)
        return self._common_prefix(left, right)
    
    def _common_prefix(self, left, right):
        minval = min(len(left), len(right))
        for i in range(minval):
            if left[i] != right[i]:
                return left[0:i]
        return left[0:minval]


def longest_common_prefix4(strs):
    """二分查找法
        复杂度分析：
            最坏情况下，我们有 n 个长度为 m 的相同字符串。
            时间复杂度：O(S*log(n))，其中 S 所有字符串中字符数量的总和。
                算法一共会进行 log(n) 次迭代，每次一都会进行 S = m*n 次比较，所以总时间复杂度为 O(S*log(n))。
            空间复杂度：O(1)，我们只需要使用常数级别的额外空间。
    """
    assert(isinstance(strs, list))
    if len(strs) == 0:
        return ''
    min_len = 2**31 - 1
    for s in strs:
        min_len = min(min_len, len(s))
    low, high = 1, min_len
    while low <= high:
        middle = (low + high) // 2
        if is_common_prefix(strs, middle):
            low = middle + 1
        else:
            high = middle - 1
    return strs[0][0:(low + high) // 2]

def is_common_prefix(strs, mid):
    s, l = strs[0][0:mid], len(strs)
    for i in range(l):
        if not strs[i].startswith(s):
            return False
    return True


strs1 = ["flower","flow","flight"]
strs2 = ["dog","racecar","car"]
strs3 = ["ccc", "ccc", "ccc"]
# print(longest_common_prefix1(strs1))
# print(longest_common_prefix1(strs2))

%time longest_common_prefix1(strs1)
%time longest_common_prefix2(strs1)
# print(longest_common_prefix2(strs1))
# print(longest_common_prefix2(strs2))
# print(longest_common_prefix2(strs3))

# Output:
# CPU times: user 9 µs, sys: 0 ns, total: 9 µs
# Wall time: 11.4 µs
# CPU times: user 77 µs, sys: 0 ns, total: 77 µs
# Wall time: 25.3 µs
# 'fl'

testcases = [strs1, strs2, strs3]
s = Solution()
for tc in testcases:
    val = s.longest_common_prefix(tc)
    print(val)
    break

# Output:
# fl

print(longest_common_prefix4(strs1))
print(longest_common_prefix4(strs2))
print(longest_common_prefix4(strs3))

# Output:
# fl
#
# ccc
```
