---
title: "A Brain Teaser in a Coding Interview"
categories: jekyll update
tags: [news]
permalink: a-brain-teaser-in-a-coding-interview.html
date: 2020-08-24 10:10:15 +0700
summary: "This post discussed a brain teaser in a coding interview. It is worth noting that while most coding interview
problems require a significant amount of coding effort; on the contrary, this problem is far from being
programming-heavy and requires analytical
thinking instead."
---

- [The Problem](#the-problem)
- [The First Round](#the-first-round)
- [The Second Round](#the-second-round)
- [The Solution](#the-solution)

Today we are discussing a typical brain teaser problem in a coding interview. Despite its simplicity, the problem is
quite challenging, especially when you are confronted with such a non-conventional brain teaser in a coding interview.

## The Problem
Suppose you had two integers ranging from 1 to 20 inclusive, and two adversaries were rivaling to guess them. You told A
the sum of the two, and told B the product of the two. Then several rounds of dialogue occurred sequentially:

> A said: "I cannot decide the two integers ..."  
> B said: "I cannot decide the two integers either ..."  
> A said: "Then I can decide the two integers ..."  
> B said: "Then I can decide the two integers too ..."

Suppose A and B both made educated inferences to the best of their knowledge gathered up to the moment of utterance.
Then can you figure out what the two integers were from their dialogue?

## The First Round
When A said he wasn't able to decide the two integers, it was quite understandable since too little information was
disclosed concerning the integers by simply providing the sum. At this time, it was obviously true that 4<=sum<=38.

As B in turn said he wasn't able to decide the two integers, however, more information was revealed. This meant there
were at least two ways to factorize the product with integers ranging between 1 and 20 inclusive.

Then A said he was able to decide the integers, which was to say situations like the following one should never happen.
For example, if the sum of integers could be 28, then we would at least have two candidate pairs (20,8) or (18,10). In
either case, B was not able to decide the integers, since the product of (20,8) could result from (10,16), and the
product of (18,10) could result from (9,20). In conclusion, we have in effect reduced the problem to finding a pair of
integers, such that given the sum, there was *exactly* one choice of product that enabled itself to be factorized in at
least two ways with integers ranging between 1 and 20 inclusive. It is high time that we exhaust all of them:

$$
\begin{array}{|c|c|c|c|}
\hline
\text{sum} & \text{possible pairs in A's view} & \text{sum} & \text{possible pairs in A's view} \\
\hline
   <=3   &                         (none)                        &     4     &                         (2, 2)                           \\
     5      &                   (1, 4), (2, 3), etc.               &     6     &                   (2, 4), (3, 3), etc.                \\
     7      &                   (1, 6), (2, 5), etc.               &     8     &                   (2, 6), (3, 5), etc.                \\
     9      &                   (1, 8), (2, 7), etc.               &    10    &                   (1, 9), (2, 8), etc.                \\
    11     &                  (1, 10), (3, 8), etc.              &    12    &                  (2, 10), (4, 8), etc.               \\
    13     &                  (1, 12), (3, 10), etc.            &    14    &                  (2, 12), (4, 10), etc.             \\
    15     &                  (1, 14), (3, 12), etc.            &    16    &                  (1, 15), (2, 14), etc.             \\
    17     &                  (1, 16), (2, 15), etc.            &    18    &                  (2, 16), (3, 15), etc.             \\
    19     &                  (1, 18), (3, 16), etc.            &    20    &                  (2, 18), (4, 16), etc.             \\
    21     &                  (1, 20), (3, 18), etc.            &    22    &                  (2, 20), (4, 18), etc.             \\
    23     &                  (3, 20), (5, 18), etc.            &    24    &                  (4, 20), (6, 18), etc.             \\
    25     &                  (5, 20), (9, 16), etc.            &    26    &                  (6, 20), (8, 18), etc.             \\
    27     &                  (7, 20), (12, 15), etc.          &    28    &                  (8, 20), (10, 18), etc.           \\
    29     &                       (9, 20)                          &    30    &                        (none)                          \\
    31     &                       (15, 16)                        &    32    &                       (12, 20)                         \\
  >=33  &                        (none)                         &            &                                                            \\
\hline
\end{array}
$$

## The Second Round
From this table, when A knew "the sum was 31" and "B was not able to decide the solution with product told", A inferred
the product had to be 240, and then the solution was (15,16). Nevertheless, B was not able to tell (15,16) from (12,20)
when he knew "the product was 240" and "A already knew the solution", since (12,20) also served this purpose. To better
demonstrate B's view, another table have to be constructed:

$$
\begin{array}{|c|c|}
\hline
\text{product} & \text{possible pairs in B's view} \\
\hline
4 & (2, 2) \\
180 & (20, 9) \\
240 & (15, 16), (12, 20) \\
\hline
\end{array}
$$

## The Solution
From this table, B was able to infer the solution for products 4 and 180. Therefore, the possible answers to this puzzle
are (20,9) and (2,2). To emphasize, we should never miss the simpler solution (2,2), which is permissible if we don't
explicitly require the integers to be distinct.
