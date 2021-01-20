---
title: "Friendly Arrays"
keywords: friendly array
date: 2020-01-22 22:14:35 -0800
last_updated: December 26, 2020
tags: [algorithms]
summary: "Today we are discussing a very interesting algorithmic problem called the friendly arrays."
sidebar: none
permalink: friendly_arrays.html
folder: mydoc
---

Today we are discussing a very interesting algorithmic problem called the friendly arrays.

## Problem Formulation
Informally, we define the $k$-th friendly array to be one integer array of length $2k$, in which each integer lies in
the range $[1,k]$ and appears exactly twice, in an manner that each pair of $i$'s are spaced by $i$ integers,
$i\in[1,k]$. Let us enumerate the first few friendly arrays, for example:

```
 the $n$-th ordinal | the $n$-th friendly arrays                                        |
 the 1-st           | (none)                                                            |
 the 2-nd           | (none)                                                            |
 the 3-rd           | (3, 1, 2, 1, 3, 2), (2, 3, 1, 2, 1, 3)                            |
 the 4-th           | (2, 3, 4, 2, 1, 3, 1, 4), (4, 1, 3, 1, 2, 4, 3, 2)                |
 the 5-th           | (none)                                                            |
 the 6-th           | (none)                                                            |
 the 7-th           | (4, 6, 1, 7, 1, 4, 3, 5, 6, 2, 3, 7, 2, 5), and 51 more...        |
 the 8-th           | (6, 2, 8, 5, 2, 4, 7, 6, 3, 5, 4, 8, 3, 1, 7, 1), and 299 more... |
```
<center><font face="Lora">Table 1: The Friendly Arrays For The First Few Ordinals</font></center>

Then what is the systematic procedure to generate (preferably in an exhaustive manner) such $k$-th friendly arrays? In
the very beginning, I was tempted to crack this problem by following the tricks applied to the
[<font face="Lora">Josephus Problem</font>](#references). However, due to subtle differences, all efforts were in vain.
Vicariously, it suddenly dawned on me that the friendly arrays might be approached from a pure programming perspective
with stacks, and this opinion indeed worked out. In the rest of the post, let us illustrate this approach for the
$8$-th ordinal.

## Stack Version One
Without considerations for memory efficiency, this straight-forward Stack Version One implements a
<font face="Lora">breadth-first-search (BFS)</font> approach using a stack, which consumes the most memory space.
Supposing for the $n$-th ordinal, the number of the $n$-th friendly arrays in total is given by $f(n)$. More to come to
estimate time and space complexity in details ...

<center>
    <img src="{{ "images/20200122-2.png" }}" alt="Breadth-First-Search"/>
    <font face="Lora">Figure 1: Illustration Of Stack Details In Breadth-First-Search.</font>
</center>

```python
# Friendly Arrays Stack Version One

"""
n:      specify n for n-th friendly arrays
stack:  stack to pack in unexplored status, where 'filled' is (partly) filled array, 'cur_val' is last value filled.
cnt:    counter to store number of n-th friendly arrays
"""

n = 8
stack = [{'filled': [0] * 2 * n, 'cur_val': n + 1}]
cnt = 0

while stack:
    array, cur_val = stack[-1]['filled'], stack[-1]['cur_val'] - 1
    stack = stack[:-1]
    if cur_val == 0:    # already have filled 1 so that 0 is next to fill, and you are done.
        cnt += 1
        print('The {}-th friendly array is {}'.format(cnt, array))
        continue
    for pos in range(2 * n - cur_val - 1):
        if array[pos] == 0 and array[pos + cur_val + 1] == 0:
            arr = array[:]  # clone by slicing
            arr[pos], arr[pos + cur_val + 1] = cur_val, cur_val
            stack.append({'filled': arr, 'cur_val': cur_val})
print('There are in total {} friendly arrays corresponding to the {}-th ordinal.'.format(cnt, n))
```

The complete friendly arrays are:
```
The 1-th friendly array is [1, 5, 1, 4, 6, 7, 8, 5, 4, 2, 3, 6, 2, 7, 3, 8]
The 2-th friendly array is [1, 5, 1, 6, 4, 7, 8, 5, 3, 4, 6, 2, 3, 7, 2, 8]
The 3-th friendly array is [3, 5, 6, 4, 3, 7, 8, 5, 4, 6, 1, 2, 1, 7, 2, 8]
...
There are in total 300 friendly arrays corresponding to the 8-th ordinal.
```

## Stack Version Two
With considerations for memory efficiency, this slightly improved Stack Version Two approach implements a
<font face="Lora">Depth-First-Search (DFS)</font> still using a stack, which consumes much less memory space. At any
time, the stack at most unfolds one status for each level down through the hierarchy, essentially trading time
complexity for space complexity. Supposing for the $n$-th ordinal, the number of the $n$-th friendly arrays in total is
given by $f(n)$. More to come to estimate time and space complexity in details ...

<center>
    <img src="{{ "images/20200122-3.png" }}" alt="Depth-First-Search"/>
    <font face="Lora">Figure 2: Illustration Of Stack Details In Depth-First-Search.</font>
</center>

```python
# Friendly Arrays Stack Version Two

"""
n:      specify n for n-th friendly arrays
stack:  stack to pack in unexplored status, where 'filled' is (partly) filled array, 'cur_val' is last value filled.
cnt:    counter to store number of n-th friendly arrays
"""

n = 8
stack = [{'filled': [0] * 2 * n, 'cur_val': n}]
stack[0]['filled'][0], stack[0]['filled'][n + 1] = n, n
cnt = 0

def next_stack(stack):
    array, cur_val = stack[-1]['filled'], stack[-1]['cur_val'] - 1
    # if inserting cur_val is possible, then return.
    for i in range(2 * n - cur_val - 1):
        if array[i] == 0 and array[i + cur_val + 1] == 0:
            arr = array[:]
            arr[i], arr[i + cur_val + 1] = cur_val, cur_val
            return stack + [{'filled': arr, 'cur_val': cur_val}]
    # else, inserting cur_val is not possible, then pop from stack until find next status and return.
    while stack:
        array, cur_val = stack[-1]['filled'], stack[-1]['cur_val']
        pos = array.index(cur_val)
        array[pos], array[pos + cur_val + 1] = 0, 0
        for i in range(pos + 1, 2 * n - cur_val - 1):
            if array[i] == 0 and array[i + cur_val + 1] == 0:
                array[i], array[i + cur_val + 1] = cur_val, cur_val  # modify array itself in place, no need to clone.
                return stack
        stack = stack[:-1]
    # else, return [].
    return stack

while stack:
    array, cur_val = stack[-1]['filled'], stack[-1]['cur_val'] - 1
    if cur_val == 0:    # already have filled 1 so that 0 is next to fill, and you are done.
        cnt += 1
        print('The {}-th friendly array is {}'.format(cnt, array))
    stack = next_stack(stack)
print('There are in total {} friendly arrays corresponding to the {}-th ordinal.'.format(cnt, n))
```

The complete friendly arrays are:

```
The 1-th friendly array is [8, 3, 7, 2, 6, 3, 2, 4, 5, 8, 7, 6, 4, 1, 5, 1]
The 2-th friendly array is [8, 2, 7, 3, 2, 6, 4, 3, 5, 8, 7, 4, 6, 1, 5, 1]
The 3-th friendly array is [8, 2, 7, 1, 2, 1, 6, 4, 5, 8, 7, 3, 4, 6, 5, 3]
...
There are in total 300 friendly arrays corresponding to the 8-th ordinal.
```

## Iterative Version
With more concern for memory efficiency, this largely improved Iterative Version approach (using no stack) consumes only
constant $O(1)$ memory space, i.e. an in-place algorithm. At any time, only the current status is maintained. It uses as
low a space complexity as possible. Supposing for the $n$-th ordinal, the number of the $n$-th friendly arrays in total
is given by $f(n)$. More to come to estimate time and space complexity in details ...

```python
# Friendly Arrays Iterative Version

"""
n:      specify n for n-th friendly arrays
status: current status. 'filled' is (partly) filled array, 'cur_val' is last value filled.
cnt:    counter to store number of n-th friendly arrays
"""

n = 8
status = {'filled': [0] * 2 * n, 'cur_val': n + 1}
cnt = 0

def next_status(status):
    array, cur_val = status['filled'], status['cur_val'] - 1
    # if inserting cur_val is possible, then return.
    for i in range(2 * n - cur_val - 1):
        if array[i] == 0 and array[i + cur_val + 1] == 0:
            array[i], array[i + cur_val + 1] = cur_val, cur_val
            return {'filled': array, 'cur_val': cur_val}
    # else, inserting cur_val is not possible, then pop from stack until find next status and return.
    cur_val += 1
    while cur_val <= n:
        pos = array.index(cur_val)
        array[pos], array[pos + cur_val + 1] = 0, 0
        for i in range(pos + 1, 2 * n - cur_val - 1):
            if array[i] == 0 and array[i + cur_val + 1] == 0:
                array[i], array[i + cur_val + 1] = cur_val, cur_val  # modify array itself in place, no need to clone.
                status['cur_val'] = cur_val
                return status
        cur_val += 1
    # else, return {}.
    return {}

while status:
    array, cur_val = status['filled'], status['cur_val'] - 1
    if cur_val == 0:    # already have filled 1 so that 0 is next to fill, and you are done.
        cnt += 1
        print('The {}-th friendly array is {}'.format(cnt, array))
    status = next_status(status)
print('There are in total {} friendly arrays corresponding to the {}-th ordinal.'.format(cnt, n))
```

The complete friendly arrays are:

```
The 1-th friendly array is [8, 3, 7, 2, 6, 3, 2, 4, 5, 8, 7, 6, 4, 1, 5, 1]
The 2-th friendly array is [8, 2, 7, 3, 2, 6, 4, 3, 5, 8, 7, 4, 6, 1, 5, 1]
The 3-th friendly array is [8, 2, 7, 1, 2, 1, 6, 4, 5, 8, 7, 3, 4, 6, 5, 3]
...
There are in total 300 friendly arrays corresponding to the 8-th ordinal.
```

## References
Ronald L. Graham, Donald E. Knuth, and Oren Patashnik. 1994. Concrete Mathematics (Second Edition). Chapter 1.3.

{% include links.html %}
