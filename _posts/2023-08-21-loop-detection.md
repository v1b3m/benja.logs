---
title: "Loop Detection"
date: "2023-08-21T09:10:48+0300"
categories:
  - dsa
tags:
  - linked lists
  - loop detection
toc: true
header:
  image: /assets/images/00090-2735675439.jpg
---

## PROBLEM

Given a circular linked list, implement an algorithm that returns the node at the beginning of the loop.

### DEFINITION

Circular linked list: A (corrupt) linked list in which a node's next pointer points to an earlier node, so as to make a loop in the linked list.

### EXAMPLE

Input: A -> B -> C -> D -> E -> C [the same C as earlier]

Output: C

### HINTS
<!-- primary/info/warning/success/danger -->

**#50:** There are really two parts to this problem. First, detect if the linked list has a loop. Second, figure out where the loop starts.
{: .notice--primary}

**#69:** To identify if there's a cycle, try the "runner" approach described on page 93. Have one pointer move faster than the other.
{: .notice--info}

**#83:** You can use two pointers, one moving twice as fast as the other. If there is a cycle, the two pointers will collide. They will land at the same location at the same time.Where do they land? Why there?
{: .notice--warning}

**#90:** If you haven't identified the pattern of where the two pointers start, try this: Use the linked list 1->2->3->4->5->6->7->8->9->?,
where the ? links to another node. Try making the ? the first node (that is, the 9 points to the 1 such that the entire linked list is a loop). Then make the ? the node 2. Then
the node 3. Then the node 4. What is the pattern? Can you explain why this happens?
{: .notice--danger}

Hints: #50, #69, #83, #90 

## SOLUTION

Solving the "Loop Detection" problem involves detecting if there's a loop in a circular linked list and then finding the node where the loop starts. Here's a step-by-step guide for a beginner:

1. **Understand the Problem:**
   Read and understand the problem statement carefully. Make sure you comprehend the concepts of linked lists and loops within them.

2. **Visualize the Circular Linked List:**
   Visualize the circular linked list in your mind or on paper. Understand that a circular linked list has a node where the "next" pointer points back to a previous node, forming a loop.

3. **Using Two Pointers (Floyd's Tortoise and Hare Algorithm):**
   The key idea here is to use two pointers, one moving faster than the other, to detect a loop. This algorithm is also known as Floyd's Tortoise and Hare Algorithm.

4. **Initialize Pointers:**
   Initialize two pointers, one called "slow" and the other "fast," both pointing to the head of the linked list.

5. **Detect Loop:**
   Traverse the linked list in a loop using the pointers. The "slow" pointer moves one step at a time, and the "fast" pointer moves two steps at a time. If there's a loop, the fast pointer will eventually catch up to the slow pointer.

6. **Check for Loop:**
   While traversing, if the fast pointer becomes equal to the slow pointer or if it reaches the end of the list, you've either detected a loop or reached the end of the list without finding a loop.

7. **Find the Start of the Loop:**
   If a loop is detected, reset one of the pointers (let's say "slow") to the head of the linked list. Then, move both pointers (slow and fast) one step at a time until they meet again. The node at which they meet is the start of the loop.

8. **Return the Start Node:**
   Return the node where the two pointers met. This is the node at the beginning of the loop.

9. **Code Implementation:**
   Implement the above algorithm in your programming language of choice. Use classes or structures to represent linked list nodes, and write functions to perform the loop detection and finding the loop's starting node.

10. **Test Your Code:**
    Create circular linked lists with loops for testing. Test your code with different inputs to ensure it's working correctly. Verify that the returned node is indeed the start of the loop.

11. **Optimize and Refactor:**
    Review your code for any potential optimizations or improvements. Make sure your code is clean, well-organized, and easy to understand.

12. **Document Your Solution:**
    Write comments in your code to explain each step and the logic behind it. This will make your code more readable for others and for future reference.

By following these steps, you'll be able to effectively solve the "Loop Detection" problem and find the node at the beginning of the loop in a circular linked list.

### When you reset the "slow" pointer to the start, why is the node at which they meet, the node at the start of the loop. What's the logic behind this?

{% include figure image_path="/assets/images/dsa/IMG_F2A7F9004CCF-1.jpg" alt="this is a placeholder image" caption="This is a figure caption." %}

The logic behind resetting the "slow" pointer to the head of the linked list and then moving both the "slow" and "fast" pointers one step at a time until they meet again lies in the properties of the circular linked list and the relative distances traveled by the two pointers.

Let's break down the logic step by step:

1. **Distance to the Loop Start:**
   When the "slow" pointer and the "fast" pointer meet, they are both at some point within the loop. Let's assume that the distance from the head of the linked list to the start of the loop is `x`, and the distance from the start of the loop to the meeting point is `y`.

2. **Fast Pointer's Distance:**
   The "fast" pointer travels twice as fast as the "slow" pointer. Therefore, by the time they meet, the "fast" pointer has traveled twice the distance of the "slow" pointer. So, the distance traveled by the "fast" pointer can be represented as `2 * (x + y)`.

3. **Slow Pointer's Distance:**
   The "slow" pointer has traveled a distance of `x + y` to reach the meeting point.

4. **Distance Covered within the Loop:**
   Since the "fast" pointer has traveled twice the distance of the "slow" pointer within the loop, the remaining distance to complete the loop for the "fast" pointer is also `x + y`.

5. **Why Reset "Slow" to the Head:**
   By resetting the "slow" pointer to the head of the linked list and then moving both pointers one step at a time, they effectively start moving towards each other again. Since both pointers are now `x` distance away from the loop start, they will meet exactly at the start of the loop.

In essence, the logic relies on the relative distances covered by the "slow" and "fast" pointers within the loop and how they behave when they move towards each other. By the time they meet after resetting the "slow" pointer, they are both `x` distance away from the loop start, resulting in them meeting at the start of the loop. This property holds true for any point within the loop, making it a reliable technique for finding the node at the beginning of the loop.

## CODE

```ts
class ListNode {
  value: number;
  next: ListNode | null;
  constructor(value: number) {
    this.value = value;
    this.next = null;
  }
}

function detectLoopStart(
  head: ListNode | null
): ListNode | null {
  if (!head || !head.next) {
    // No loop if the list is empty or has only one node
    return null; 
  }

  let slow = head;
  let fast = head;

  // Move slow by 1 step and fast by 2 steps
  while (fast !== null && fast.next !== null) {
    slow = slow.next!;
    fast = fast.next.next!;

    if (slow === fast) {
      // Loop detected
      break;
    }
  }

  if (slow !== fast) {
    return null; // No loop found
  }

  // Reset slow to head and move both pointers
  // 1 step at a time until they meet
  slow = head;
  while (slow !== fast) {
    slow = slow.next!;
    fast = fast.next!;
  }

  return slow; // Node at the beginning of the loop
}
```
