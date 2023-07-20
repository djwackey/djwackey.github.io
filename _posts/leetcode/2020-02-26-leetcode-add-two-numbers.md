---
layout: post
title: Leetcode刷题 - 两数相加
author: djwackey
tags: [Leetcode]
categories: leetcode
excerpt_separator: <!--more-->
---

# 2.两数相加

难度：中

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。<br>
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。<br>
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。<br>

示例：

输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)<br>
输出：7 -> 0 -> 8<br>
原因：342 + 465 = 807<br>
<!--more-->


```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


def add_two_numbers(l1, l2):
    """两数相加
        复杂度分析：
            时间复杂度：O(max(m,n))，假设 m 和 n 分别表示 l1 和 l2 的长度，上面的算法最多重复max(m,n)次。
            空间复杂度：O(max(m,n))，新列表的长度最多为max(m,n)+1。
    """
    assert(l1 is None or isinstance(l1, ListNode))
    assert(l2 is None or isinstance(l2, ListNode))
    p, q, carry = l1, l2, 0
    curr = dummy_head = ListNode(0)
    while p != None or q != None:
        x = p.val if p != None else 0
        y = q.val if q != None else 0
        s = carry + x + y
        carry = s // 10
        curr.next = ListNode(s % 10)
        curr = curr.next
        if p != None:
            p = p.next
        if q != None:
            q = q.next
    if carry > 0:
        curr.next = ListNode(carry)
    return dummy_head.next

def make_linked_list(values):
    """转换列表到链表
    """
    n = None
    for v in values:
        if n is None:
            n = ListNode(v)
            t = n
        else:
            t.next = ListNode(v)
            t = t.next
    return n

def print_linked_list(l):
    """打印链表
    """
    assert(l is None or isinstance(l, ListNode))
    while l is not None:
        print(l, l.__dict__)
        l = l.next

def show_two_numbers(l1, l2):
    """显示两数相加过程
    """
    print_linked_list(l1)
    print('-' * 100)
    print_linked_list(l2)
    print('-' * 100)
    result = add_two_numbers(l1, l2)
    print_linked_list(result)
    return result

def main():
    """
    Output:
        <__main__.ListNode object at 0x7fa41832d6a0> {'val': 2, 'next': <__main__.ListNode object at 0x7fa41832d630>}
        <__main__.ListNode object at 0x7fa41832d630> {'val': 4, 'next': <__main__.ListNode object at 0x7fa41832d470>}
        <__main__.ListNode object at 0x7fa41832d470> {'val': 3, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d550> {'val': 5, 'next': <__main__.ListNode object at 0x7fa41832df98>}
        <__main__.ListNode object at 0x7fa41832df98> {'val': 6, 'next': <__main__.ListNode object at 0x7fa418378f98>}
        <__main__.ListNode object at 0x7fa418378f98> {'val': 4, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41839ad30> {'val': 7, 'next': <__main__.ListNode object at 0x7fa418378a20>}
        <__main__.ListNode object at 0x7fa418378a20> {'val': 0, 'next': <__main__.ListNode object at 0x7fa4183e7160>}
        <__main__.ListNode object at 0x7fa4183e7160> {'val': 8, 'next': None}
        
        
        <__main__.ListNode object at 0x7fa41832d208> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832d4e0>}
        <__main__.ListNode object at 0x7fa41832d4e0> {'val': 1, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d630> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832d470>}
        <__main__.ListNode object at 0x7fa41832d470> {'val': 1, 'next': <__main__.ListNode object at 0x7fa41832d2b0>}
        <__main__.ListNode object at 0x7fa41832d2b0> {'val': 2, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d550> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832df98>}
        <__main__.ListNode object at 0x7fa41832df98> {'val': 2, 'next': <__main__.ListNode object at 0x7fa41832d6a0>}
        <__main__.ListNode object at 0x7fa41832d6a0> {'val': 2, 'next': None}
        
        
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d4e0> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41839ad30>}
        <__main__.ListNode object at 0x7fa41839ad30> {'val': 1, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d470> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832d2b0>}
        <__main__.ListNode object at 0x7fa41832d2b0> {'val': 1, 'next': None}
        
        
        <__main__.ListNode object at 0x7fa41832df98> {'val': 9, 'next': <__main__.ListNode object at 0x7fa41832d6a0>}
        <__main__.ListNode object at 0x7fa41832d6a0> {'val': 9, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d630> {'val': 1, 'next': None}
        ----------------------------------------------------------------------------------------------------
        <__main__.ListNode object at 0x7fa41832d4e0> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832d550>}
        <__main__.ListNode object at 0x7fa41832d550> {'val': 0, 'next': <__main__.ListNode object at 0x7fa41832d208>}
        <__main__.ListNode object at 0x7fa41832d208> {'val': 1, 'next': None}
    """
    testcases = [
        ([2, 4, 3], [5, 6, 4]),
        ([0, 1], [0, 1, 2]),
        ([], [0, 1]),
        ([9, 9], [1])
    ]
    for vs1, vs2 in testcases:
        l1 = make_linked_list(vs1)
        l2 = make_linked_list(vs2)
        _ = show_two_numbers(l1, l2)
        print('\n')


if __name__ == "__main__":
    main()
```
