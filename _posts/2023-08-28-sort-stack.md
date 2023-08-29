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

To solve this problem, we can use two stacks: the main stack (let's call it `s1`), which we need to sort, and a temporary stack (let's call it `s2`) which will assist in sorting.

The idea is similar to the insertion sort:

1. While `s1` is not empty, pop the top element from `s1` (let's call it `temp`).
2. Compare `temp` with the top of `s2`.
   a. If `s2` is empty or `temp` is less than the top of `s2`, push `temp` onto `s2`.
   b. If `temp` is greater than the top of `s2`, pop the top from `s2` and push it onto `s1`. Repeat this step until condition 2a is met, then push `temp` onto `s2`.
3. Once `s1` is empty, `s2` will have the sorted elements with the smallest on top.

Here's the code to implement this:

```typescript
class Stack {
  private items: number[] = [];

  push(value: number): void {
    this.items.push(value);
  }

  pop(): number | null {
    return this.items.pop() || null;
  }

  peek(): number | null {
    return this.items.length ? this.items[this.items.length - 1] : null;
  }

  isEmpty(): boolean {
    return !this.items.length;
  }
}

function sortStack(s1: Stack): Stack {
  const s2 = new Stack();

  while (!s1.isEmpty()) {
    const temp = s1.pop();

    while (!s2.isEmpty() && s2.peek()! > temp!) {
      s1.push(s2.pop()!);
    }

    s2.push(temp!);
  }

  return s2; // s2 is now the sorted stack
}

// Example usage:
const stack = new Stack();
stack.push(3);
stack.push(1);
stack.push(4);
stack.push(2);

const sortedStack = sortStack(stack);
while (!sortedStack.isEmpty()) {
  console.log(sortedStack.pop()); // It will print: 1, 2, 3, 4
}
```

The `sortStack` function takes in a stack and returns a new stack that is sorted with the smallest elements on top. Note that the original stack will be emptied in this process.
