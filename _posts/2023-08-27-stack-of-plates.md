---
title: "Stack Of Plates"
categories:
  - dsa
  - problem
tags:
  - stack
toc: true
header:
  image: /assets/images/dsa/00105-189411473.jpg
---

## PROBLEM

Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure `SetOfStacks` that mimics this. `SetOfStacks` should be composed of several stacks and should create a new stack once the previous one exceeds capacity. `SetOfStacks.push()` and `SetOfStacks.pop()` should behave identically to a single stack (that is, `pop()` should return the same values as it would if there were just a single stack).

### FOLLOW UP

Implement a function `popAt(int index)` which performs a pop operation on a specific sub-stack.

### HINTS

**#64** You will need to keep track of the size of each substack. When one stack is full, you may need to create a new stack
{: .notice--primary}

**#81** Popping an element at a specific substack will mean that some stacks aren't at full capacity. Is this an issue? There's no right answer, but you should think about how to handle this.
{: .notice--success}

## SOLUTION

Alright, let's break down the problem.

We'll have a `SetOfStacks` class which is basically a list of individual stacks (sub-stacks). Each of these sub-stacks will have a capacity (limit), and once this limit is reached, we need to create a new sub-stack to store more plates.

### Steps:
1. Initialize `SetOfStacks` with an empty list of sub-stacks and a given capacity for these sub-stacks.
2. For `push`, add the plate to the latest sub-stack. If this sub-stack is full or doesn't exist, create a new sub-stack.
3. For `pop`, remove the plate from the latest sub-stack. If this sub-stack becomes empty, remove it.
4. For `popAt`, pop the plate from a specific sub-stack at the given index. As per the second hint, popping from a specific sub-stack might leave it not at full capacity, but we'll accept this behavior and won't shift elements between sub-stacks (for simplicity).

Let's implement this:

```typescript
class SetOfStacks {
  private stacks: number[][] = [];
  private readonly capacity: number;

  constructor(capacity: number) {
    this.capacity = capacity;
  }

  push(value: number): void {
    if (
      !this.stacks.length ||
      this.stacks[this.stacks.length - 1].length === this.capacity
    ) {
      // If there are no stacks or the latest stack is full, start a new stack.
      this.stacks.push([]);
    }
    this.stacks[this.stacks.length - 1].push(value);
  }

  pop(): number | null {
    if (!this.stacks.length) return null;

    const poppedValue = this.stacks[this.stacks.length - 1].pop();

    // If after popping, the latest stack is empty, remove it.
    if (!this.stacks[this.stacks.length - 1].length) {
      this.stacks.pop();
    }

    return poppedValue;
  }

  popAt(index: number): number | null {
    if (index < 0 || index >= this.stacks.length || !this.stacks[index].length)
      return null;

    const poppedValue = this.stacks[index].pop();

    // If after popping, the specified stack is empty, remove it.
    if (!this.stacks[index].length) {
      this.stacks.splice(index, 1);
    }

    return poppedValue;
  }
}
```

### Usage:

```typescript
const plates = new SetOfStacks(2); // Each stack can hold up to 2 plates.

plates.push(1);
plates.push(2);
plates.push(3);
plates.push(4);

console.log(plates.popAt(0));  // Expected: 2
console.log(plates.pop());    // Expected: 4
console.log(plates.pop());    // Expected: 3
console.log(plates.pop());    // Expected: 1
```

Here, `SetOfStacks` manages the sub-stacks and maintains the illusion of a single unified stack for the user. The follow-up function, `popAt`, allows popping from specific sub-stacks.

### What if we had to shift elements after popping if the stack is not at full capacity?

If we have to shift elements to ensure that, after popping, all stacks except the last one are at full capacity, then we need to make some modifications. Here's a plan:

After a `popAt(index)` operation which results in a sub-stack not being full:

1. We pull the bottom plate from the sub-stack at `index + 1` (if it exists) and push it to the sub-stack at `index`.
2. Continue this process for each subsequent stack until the last sub-stack, ensuring every stack is kept at full capacity.
3. If the last stack becomes empty due to this process, remove it.

Now, let's modify the `popAt` method of our previous `SetOfStacks` to implement this behavior:

```typescript
class SetOfStacks {
  private stacks: number[][] = [];
  private readonly capacity: number;

  constructor(capacity: number) {
    this.capacity = capacity;
  }

  push(value: number): void {
    if (
      !this.stacks.length ||
      this.stacks[this.stacks.length - 1].length === this.capacity
    ) {
      this.stacks.push([]);
    }
    this.stacks[this.stacks.length - 1].push(value);
  }

  pop(): number | null {
    if (!this.stacks.length) return null;

    const poppedValue = this.stacks[this.stacks.length - 1].pop();

    if (!this.stacks[this.stacks.length - 1].length) {
      this.stacks.pop();
    }

    return poppedValue;
  }

  popAt(index: number): number | null {
    if (index < 0 || index >= this.stacks.length || !this.stacks[index].length)
      return null;

    const poppedValue = this.stacks[index].pop();

    this.shiftPlatesRight(index);

    if (!this.stacks[this.stacks.length - 1].length) {
      this.stacks.pop();
    }

    return poppedValue;
  }

  // Helper function to shift plates to ensure full stacks.
  private shiftPlatesRight(index: number): void {
    if (index < 0 || index >= this.stacks.length - 1) return;

    // Pull from the next stack and push to the current stack.
    const pulledPlate = this.stacks[index + 1].shift();

    if (pulledPlate !== undefined) {
      this.stacks[index].push(pulledPlate);
    }

    // If after pulling, the next stack becomes empty, remove it.
    if (!this.stacks[index + 1].length) {
      this.stacks.splice(index + 1, 1);
    } else {
      // Otherwise, continue the process for the next stack.
      this.shiftPlatesRight(index + 1);
    }
  }
}
```

This approach ensures that, after a `popAt(index)` operation, all stacks before the last are kept at full capacity. However, this modification makes the `popAt` operation more expensive, potentially being \(O(n)\) where \(n\) is the number of sub-stacks, as it might have to shift plates across all sub-stacks.
