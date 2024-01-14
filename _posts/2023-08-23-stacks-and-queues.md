---
title: "Stacks and Queues"
categories:
  - dev
tags:
  - stacks
  - queues
  - dsa
toc: true
---

## Stacks

### Stack Implementation

```ts
class EmptyStackError implements Error {
  message: string;
  name = "EmptyStackError";

  constructor(message = "Empty Stack") {
    this.message = message;
  }
}

class StackNode<T> {
  data: T;
  next: StackNode<T>;

  constructor(data: T) {
    this.data = data;
  }
}

class MyStack<T> {
  private top: StackNode<T>;

  pop(): T {
    if (!this.top) throw new EmptyStackError();
    const item = this.top.data;

    this.top = this.top.next;
    return item;
  }

  push(item: T) {
    const t = new StackNode(item);
    t.next = this.top;
    this.top = t;
  }

  peek(): T {
    if (!this.top) throw new EmptyStackError();
    return this.top.data;
  }

  isEmpty() {
    return !this.top;
  }
}

const stack = new MyStack<number>()
stack.push(3)
stack.pop()
console.log(stack.isEmpty()) // true
stack.push(5)
stack.push(8)
const val = stack.peek()
console.log(val) // 8
console.log(stack.isEmpty()) // false
```

## Queues

### Queue Implementation
