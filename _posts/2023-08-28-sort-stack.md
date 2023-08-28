---
title: "Sort Stack"
categories:
  - dsa
  - problem
tags:
  - stack
  - sort
toc: true
header:
  image: /assets/images/dsa/00113-2615830643.jpg
---

## PROBLEM

Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: `push`, `pop`, `peek`, and `isEmpty`.

### HINTS

**#15** One way of sorting an array is to iterate through the array and insert each element into a new array in sorted order. Can you do this with a stack?
{: .notice}

**#32** Imagine your secondary stack is sorted. Can you insert elements into it in sorted order? You might need some extra storage. What could you use for extra storage?
{: .notice--warning}

**#43** Keep the secondary stack in sorted order, with the biggest elements on the top. Use the primary stack for additional storage.
{: .notice--success}

## SOLUTION
