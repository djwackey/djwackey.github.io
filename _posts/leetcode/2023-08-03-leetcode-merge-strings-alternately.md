---
layout: post
title: Leetcode刷题 - 1768.交替合并字符串
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 1768.交替合并字符串

难度：简单

给你两个字符串 word1 和 word2 。请你从 word1 开始，通过交替添加字母来合并字符串。如果一个字符串比另一个字符串长，就将多出来的字母追加到合并后字符串的末尾。

返回 **合并后的字符串**。
<!--more-->

示例 1：

```shell
输入：word1 = "abc", word2 = "pqr"
输出："apbqcr"
解释：字符串合并情况如下所示：
word1：  a   b   c
word2：    p   q   r
合并后：  a p b q c r
```

示例 2：

```shell
输入：word1 = "ab", word2 = "pqrs"
输出："apbqrs"
解释：注意，word2 比 word1 长，"rs" 需要追加到合并后字符串的末尾。
word1：  a   b 
word2：    p   q   r   s
合并后：  a p b q   r   s
```

示例 3：

```shell
输入：word1 = "abcd", word2 = "pq"
输出："apbqcd"
解释：注意，word1 比 word2 长，"cd" 需要追加到合并后字符串的末尾。
word1：  a   b   c   d
word2：    p   q 
合并后：  a p b q c   d
```

提示：
>1 <= word1.length, word2.length <= 100<br>
>word1 和 word2 由小写英文字母组成

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        i = j = 0
        result = []
        word1_len, word2_len = len(word1), len(word2)

        while i < word1_len or j < word2_len:
            if i < word1_len:
                result.append(word1[i])
                i += 1
            if j < word2_len:
                result.append(word2[j])
                j += 1

        return "".join(result)
```

运行测试用例：

```python
def test_merge_strings_alternately():
    testcases = [
        {
            "word1": "abc",
            "word2": "pqr",
            "output": "apbqcr",
        },
        {
            "word1": "ab",
            "word2": "pqrs",
            "output": "apbqrs",
        },
        {
            "word1": "abcd",
            "word2": "pq",
            "output": "apbqcd",
        },
    ]

    s = Solution()

    for tc in testcases:
        ret = s.mergeAlternately(tc["word1"], tc["word2"])
        assert ret == tc["output"]
```
