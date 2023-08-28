---
title: "Queue via Stacks"
date: "2023-08-27T20:34:48+0300"
categories:
  - dsa
  - problem
tags:
  - stack
  - queue
toc: true
---

## PROBLEM

Implement a `MyQueue` class which implements a queue using two stacks.

### HINTS

**#98** The major difference between a queue and a stack is the order of elements. A queue removes the oldest item and a stack removes the newest item. How could you remove the oldest item from a stack if you only had access to the newest item?
{: .notice--primary}

**#114** We can remove the oldest item from a stack by repeatedly removing the newest item (inserting those into the temporary stack) until we get down to one element. Then, after we've retrieved the newest item, putting all the elements back. The issue with this is that doing several pops in a row will require `O(N)` work each time. Can we optimize for scenarios where we might do several pops in a row?
{: .notice--success}

## SOLUTION

Implementing a queue using two stacks is a classic problem in computer science. The idea is to use one stack for enqueue operations and the other stack for dequeue operations. Here's how you can do it:

1. **Enqueue**: For enqueue operation, simply push the element into the first stack.
2. **Dequeue**: If the second stack is empty, pop all elements from the first stack and push them onto the second stack. Then, the top of the second stack will be the oldest element, and you can simply pop it. If the second stack is not empty, you can directly pop from it.

Here's a Python implementation of a queue using two stacks:

```typescript
class MyQueue<T> {
  private stack1: T[] = [];
  private stack2: T[] = [];

  enqueue(item: T): void {
    this.stack1.push(item);
  }

  dequeue(): T | undefined {
    if (this.stack2.length === 0) {
      while (this.stack1.length > 0) {
        this.stack2.push(this.stack1.pop() as T);
      }
    }
    if (this.stack2.length === 0) {
      throw new Error("Queue is empty");
    }
    return this.stack2.pop();
  }

  isEmpty(): boolean {
    return this.stack1.length === 0 && this.stack2.length === 0;
  }

  size(): number {
    return this.stack1.length + this.stack2.length;
  }
}
```

Note the use of generics (`<T>`) so you can create a queue for any type of data, not just numbers.

To use this:

```typescript
const q = new MyQueue<number>();
q.enqueue(1);
q.enqueue(2);
q.enqueue(3);

console.log(q.dequeue()); // 1
console.log(q.dequeue()); // 2

q.enqueue(4);
console.log(q.dequeue()); // 3
console.log(q.dequeue()); // 4
```

The key insight here is that reversing the order of elements in a stack is equivalent to using another stack. This method, while not the most efficient for constant-time operations, demonstrates the flexibility and power of data structures.

### A somewhat deeper dive

After enqueuing `n` elements, if you dequeue:

1. All items are transferred from `stack1` to `stack2`.
2. The oldest element is then removed from `stack2` when `dequeue` is called.

Now, if you enqueue a new element, it goes to `stack1`. If you call `dequeue` again immediately:

1. Since `stack2` is not empty, you'll dequeue from `stack2` without any transfers.

So, the new element you enqueued into `stack1` will not be dequeued until all elements in `stack2` have been dequeued. This is because items in `stack2` are older than items in `stack1`.

Only when `stack2` is empty and a `dequeue` operation is called will items be transferred again from `stack1` to `stack2`.

In summary, with the solution provided:

- Elements are moved from `stack1` to `stack2` only when `stack2` is empty and a dequeue operation is called.
- New enqueued items stay in `stack1` until all older items in `stack2` have been dequeued.

Thus, while it might seem that there are unnecessary moves, this strategy actually minimizes the number of transfers between the two stacks. The amortized cost for each enqueue and dequeue operation is still O(1).
