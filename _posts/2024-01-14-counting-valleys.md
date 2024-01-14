---
title: "Counting Valleys"
categories:
  - dsa
  - problem
tags:
  - dynamic programming
toc: true
header:
  overlay_color: "#333"
---

## PROBLEM

[HackerRank Link](https://www.hackerrank.com/challenges/counting-valleys/problem)

## SOLUTION

### First Attempt

Passes for most of the tests, but rather too slow on some of them.
{: .notice--primary}

```ts
function countingValleys(steps: number, path: string): number {
    // Write your code here
    let valleys = 0;
    
    let isInValley = false;
    let altitude = 0;
    
    const actions: Record<string, number> = {
        'U': 1,
        'D': -1
    }
    
    // analyze each step
    for(let step of path) {
        altitude += actions[step];
         
        if(altitude < 0 && !isInValley) {
            isInValley = true;
            valleys += 1;
        } else if(altitude >= 0) {
            isInValley = false;
        }
    }
    
    return valleys;
}
```

### Optimal Solution

```ts
function countingValleys(steps: number, path: string): number {
  let valleys = 0;
  let altitude = 0;

  // analyze each step
  for (let step of path) {
    if (step === "U") {
      altitude += 1;
      // The only way we're getting to a 0 after adding 1
      // is if we're exiting a valley: -1 + 0 = 0
      if (altitude === 0) {
        valleys += 1;
      }
    } else {
      altitude -= 1;
    }
  }

  return valleys;
}
```
