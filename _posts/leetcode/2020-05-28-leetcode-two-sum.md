---
layout: post
title: Leetcode刷题 - 1.两数之和
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 1.两数之和

难度：简单

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。<br>
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:<br>
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9<br>
所以返回 [0, 1]
<!--more-->


```python

nums = [2, 7, 11, 15]
target = 9

def two_sum1(nums, target):
    """方法一：暴力法
        复杂度分析：
            时间复杂度：O(n²)
            空间复杂度：O(1)
    """
    size = len(nums)
    for i, _ in enumerate(nums):
        for j in range(i + 1, size):
            if nums[i] + nums[j] == target:
                return [i, j]
    raise Exception('No two sum solution')

two_sum1(nums, target)
# Output: [0, 1]


def two_sum2(nums, target):
    """方法二：两遍哈希表
        复杂度分析：
            时间复杂度：O(n)
            空间复杂度：O(n)
    """
    d = {}
    for i, n in enumerate(nums):
        d[n] = i
    for i, _ in enumerate(nums):
        complement = target - nums[i]
        if complement in d and d.get(complement) != i:
            return [i, d.get(complement)]
    raise Exception('No two sum solution')

two_sum2(nums, target)
# Output: [0, 1]


def two_sum3(nums, target):
    """方法三：一遍哈希表
        复杂度分析：
            时间复杂度：O(n)
            空间复杂度：O(n)
    """
    d = {}
    for i, n in enumerate(nums):
        v = d.get(target - n) 
        if v is not None:
            return [v, i]
        d[n] = i
    raise Exception('No two sum solution')

two_sum3(nums, target)
# Output: [0, 1]
```
