---
layout: post
title: Leetcode刷题 - 无重复字符的最长子串
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 3.无重复字符的最长子串

难度：中

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

   - 输入: "abcabcbb"
   - 输出: 3 
   - 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:

   - 输入: "bbbbb"
   - 输出: 1
   - 解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:

   - 输入: "pwwkew"
   - 输出: 3
   - 解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
   - 注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
<!--more-->


### 方法1 - 暴力法:
```python
class Solution1(object):
    """暴力法
        遍历检查所有的子字符串，找出无重复的子字符串的最大长度。
        复杂度分析：
            时间复杂度：O(n³)
            空间复杂度：O(min(n, m))，我们需要 O(k) 的空间来检查子字符串中是否有重复字符，
                其中 k 表示 Set 的大小。而 Set 的大小取决于字符串 n 的大小以及字符集/字母 m 的大小。
    """
    def length_of_longest_substr(self, s):
        """计算无重复字符的最长子串的长度
        """
        assert(isinstance(s, str))
        n, ret = len(s), 0
        for i in range(n):
            for j in range(i + 1, n + 1):
                if self._all_unique(s[i:j]):
                    ret = max(ret, j - i)
        return ret
    
    def _all_unique(self, s):
        """给定字符串是否存在重复的字符
        """
        assert(isinstance(s, str))
        d = {}  # using like a set
        for ch in s:
            if ch in d:
                return False
            d[ch] = 0
        return True


l = ['abcabcbb', 'bbbbb', 'pwwkew']
obj = Solution1()
for s in l:
    # ret = %time obj.length_of_longest_substr(s)
    ret = obj.length_of_longest_substr(s)
    print('\"{}\", it\'s length of longest substr: {}'.format(s, ret))

# Output:
# "abcabcbb", it's length of longest substr: 3
# "bbbbb", it's length of longest substr: 1
# "pwwkew", it's length of longest substr: 3
```

### 方法2 - 滑动窗口:
```python
class Solution2(object):
    """滑动窗口
        复杂度分析：
            时间复杂度：O(2n) = O(n)，最糟糕情况下，每个字符将被 i 和 j 访问两次。
            空间复杂度：O(min(n, m))，与之前的方法相同。
    """
    def length_of_longest_substr(self, s):
        assert(isinstance(s, str))
        d, n, i, j, ret = {}, len(s), 0, 0, 0
        while i < n and j < n:
            if s[j] not in d:
                d[s[j]] = 0
                j += 1
                ret = max(ret, j - i)
            else:
                del d[s[i]]
                i += 1
        return ret


l = ['abcabcbb', 'bbbbb', 'pwwkew']
obj = Solution2()
for s in l:
    # ret = %time obj.length_of_longest_substr(s)
    ret = obj.length_of_longest_substr(s)
    print('\"{}\", it\'s length of longest substr: {}'.format(s, ret))

# Output:
# "abcabcbb", it's length of longest substr: 3
# "bbbbb", it's length of longest substr: 1
# "pwwkew", it's length of longest substr: 3
```

### 方法3 - 优化的滑动窗口1:
```python
class Solution3(object):
    """优化的滑动窗口1
        复杂度分析：
            时间复杂度：O(n)，索引j会迭代n次
            空间复杂度：O(min(n, m))，与之前的方法相同。
    """
    def length_of_longest_substr(self, s):
        assert(isinstance(s, str))
        d, n, i, ret = {}, len(s), 0, 0
        for j in range(n):
            # v = d.get(s[j])
            # if v is not None:
                # i = max(v, i)
            if s[j] in d:
                i = max(d[s[j]], i)
            ret = max(ret, j - i + 1)
            d[s[j]] = j + 1
        return ret


l = ['abcabcbb', 'bbbbb', 'pwwkew']
obj = Solution3()
for s in l:
    # ret = %time obj.length_of_longest_substr(s)
    ret = obj.length_of_longest_substr(s)
    print('\"{}\", it\'s length of longest substr: {}'.format(s, ret))

# Output:
# "abcabcbb", it's length of longest substr: 3
# "bbbbb", it's length of longest substr: 1
# "pwwkew", it's length of longest substr: 3
```

### 方法4 - 优化的滑动窗口2:
```python
class Solution4(object):
    """优化的滑动窗口2
        复杂度分析：
            时间复杂度：O(n)，索引j会迭代n次
            空间复杂度：O(m)，m是字符集的大小
    """
    def length_of_longest_substr(self, s):
        assert(isinstance(s, str))
        array = [0 for _ in range(128)]
        n, i, ret = len(s), 0, 0
        for j in range(n):
            i = max(array[ord(s[j])], i)
            ret = max(ret, j - i + 1)
            array[ord(s[j])] = j + 1
        return ret


l = ['abcabcbb', 'bbbbb', 'pwwkew']
obj = Solution4()
for s in l:
    # ret = %time obj.length_of_longest_substr(s)
    ret = obj.length_of_longest_substr(s)
    print('\"{}\", it\'s length of longest substr: {}'.format(s, ret))

# Output:
# "abcabcbb", it's length of longest substr: 3
# "bbbbb", it's length of longest substr: 1
# "pwwkew", it's length of longest substr: 3
```
