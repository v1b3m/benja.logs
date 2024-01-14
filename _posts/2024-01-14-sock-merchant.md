---
title: "Sock Merchant"
categories:
  - dsa
  - problem
tags:
  - dynamic programming
toc: true
header:
  overlay_color: "#E8DBC5"
---

## PROBLEM

[HackerRank Link](https://www.hackerrank.com/challenges/sock-merchant/problem)

## SOLUTION

### First Attempt

Go through the socks and increment the pairs whenever you get a one

```ts
function sockMerchant(n: number, ar: number[]): number {
  type color = number;
  type frequency = number;
  // Write your code here
  const sockMap: Record<color, frequency> = {};

  let sockPairs = 0;

  for (let num of ar) {
    const currVal = sockMap[num];
    const alreadyInMap = currVal !== undefined;

    if (alreadyInMap) {
      const nextVal = currVal + 1;
      if (nextVal == 2) {
        sockPairs += 1;
        // reset val
        sockMap[num] = 0;
      } else {
        sockMap[num] = nextVal;
      }
    } else {
      sockMap[num] = 1;
    }
  }

  return sockPairs;
}
```

### Second Attempt

Group similar colors together, sum pairs in each color

```ts
function sockMerchant(n: number, ar: number[]): number {
  type color = number;
  type frequency = number;
  // Write your code here
  const sockMap: Record<color, frequency> = {};

  let sockPairs = 0;

  for (let num of ar) {
    const currVal = sockMap[num] ?? 0;
    sockMap[num] = currVal + 1;
  }

  for (let val of Object.values(sockMap)) {
    const pairs = Math.floor(val / 2);
    sockPairs += pairs;
  }

  return sockPairs;
}
```