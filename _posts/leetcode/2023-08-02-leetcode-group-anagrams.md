---
layout: post
title: Leetcode刷题 - 字母异位词分组
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 49.字母异位词分组

难度：中等

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

字母异位词 是由重新排列源单词的所有字母得到的一个新单词。
<!--more-->

 
示例 1:

   - 输入: strs = [“eat”, “tea”, “tan”, “ate”, “nat”, “bat”]
   - 输出: [[“bat”],[“nat”,“tan”],[“ate”,“eat”,“tea”]]

示例 2:

   - 输入: strs = [“”]
   - 输出: [[“”]]

示例 3:

   - 输入: strs = ["a"]
   - 输出: [["a"]]
 

提示：

   - 1 <= strs.length <= 104
   - 0 <= strs[i].length <= 100
   - strs[i] 仅包含小写字母

```python
from typing import List


class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        table = {}
        for s in strs:
            s_ = "".join(sorted(s))
            if s_ not in table:
                table[s_] = [s]
            else:
                table[s_].append(s)
        return list(table.values())
```

运行测试用例：


```python
def test_group_anagrams():
    testcases = [
        (
            ["eat", "tea", "tan", "ate", "nat", "bat"],
            [["bat"], ["nat", "tan"], ["ate", "eat", "tea"]],
        ),
        (
            [""],
            [[""]],
        ),
        (
            ["a"],
            [["a"]],
        ),
    ]

    s = Solution()
    for strs, outs in testcases:
        ret = s.groupAnagrams(strs)
        # 检测是否与输出一致
        assert len(ret) == len(outs) and len(outs) == sum(
            [
                1
                for i, j in zip(sorted(ret, key=len), sorted(outs, key=len))
                if sorted(i) == sorted(j)
            ]
        )

# Output:
# test_group_anagrams.py::test_group_anagrams PASSED
```
