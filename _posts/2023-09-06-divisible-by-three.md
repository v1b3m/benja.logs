---
title: "Write a regular expression for numbers divisible by 3."
categories:
  - cs
tags:
  - modulo
  - arithmetic
  - regex
  - maths
toc: true
header:
  # image: /assets/images/00126-366256673.jpg
---

## SOLUTION

**#2:** No PCRE nonsense, standard regex using only `()|*` Must work for arbitrary length strings
{: .notice}

Detecting numbers divisible by 3 using standard regex is an interesting challenge.

A number is divisible by 3 if the sum of its digits is divisible by 3. We can use this rule to construct a regular expression. The most basic cases are single-digit numbers divisible by 3: `0`, `3`, `6`, and `9`. This can be represented as the regex:

```js
^(0|3|6|9)$
```

For multi-digit numbers, there are various combinations of digits that sum up to be divisible by 3, like `12` (1 + 2 = 3), `21` (2 + 1 = 3), `111` (1 + 1 + 1 = 3), `222` (2 + 2 + 2 = 6), etc.

To create a regex pattern for any length string, we can use a recursive definition using a DFA (Deterministic Finite Automaton). Here's a regex based on such an automaton:

```js
const regexPatern = /^([0369]|()|[147][0369]*[258]|([258]|[147][0369]*[147])([0369]|[258][0369]*[147])*([147]|[258][0369]*[258]))([0369]|[147][0369]*[258]|([258]|[147][0369]*[147])([0369]|[258][0369]*[147])*([147]|[258][0369]*[258]))*([0369]|()|[147][0369]*[258]|([258]|[147][0369]*[147])([0369]|[258][0369]*[147])*([147]|[258][0369]*[258]))$/gm


const brokenPattern = `
  ^
  (
    [0369]|
    ()|
    [147][0369]*[258]|
    (
      [258]|
      [147][0369]*[147]
    )
    (
      [0369]|
      [258][0369]*[147]
    )*
    (
      [147]|
      [258][0369]*[258]
    )
  )
  (
    [0369]|
    [147][0369]*[258]|
    (
      [258]|
      [147][0369]*[147]
    )
    (
      [0369]|
      [258][0369]*[147]
    )*
    (
      [147]|
      [258][0369]*[258]
    )
  )*
  (
    [0369]|
    ()|
    [147][0369]*[258]|
    (
      [258]|
      [147][0369]*[147]
    )
    (
      [0369]|
      [258][0369]*[147]
    )*
    (
      [147]|
      [258][0369]*[258]
    )
  )
  $
`.replace(/\n\s+/g, "");

const regex = new RegExp(brokenPattern, 'gm');
```

**#2:** Copy and Paste the regex into [regex101](https://regex101.com/) or [regexr](https://regexr.com/) to see its breakdown and explanation. These sites will also allow you to run tests on the regex.
{: .notice--primary}

Here's how this pattern works:

1. The automaton has 3 states, corresponding to numbers modulo 3 (0, 1, 2).
2. The regex starts and ends in the state corresponding to 0 modulo 3 (i.e., the number is divisible by 3).
3. Digits are processed one at a time, transitioning the automaton from one state to another.
4. Digits `0`, `3`, `6`, and `9` don't change the modulo state.
5. Digits `1` and `4` move the automaton from state 0 to state 1, from state 1 to state 2, and from state 2 to state 0.
6. Digits `2`, `5`, and `8` move the automaton from state 0 to state 2, from state 2 to state 1, and from state 1 to state 0.

{% include figure image_path="/assets/images/F5QKt_ZaEAAjqS5.png" alt="Signals and threads" %}

The provided regex pattern tries to capture this state transition behavior for any length string.

However, remember that regex is not always the most efficient or practical tool for such tasks. In many cases, using a short script or program to evaluate divisibility might be simpler and more efficient.

## A DEEPER DIVE

Let's break this down step by step.

### Basic Concept

To determine if a number is divisible by 3, you sum its digits. If the sum is divisible by 3, then the number itself is also divisible by 3.

For example, consider the number `123`. Its digits are `1`, `2`, and `3`. Their sum is 6. Since 6 is divisible by 3, the number `123` is also divisible by 3.

### States and Transitions

The approach with the regex is to treat the divisibility by 3 check as a state machine with three states:

1. The current sum is 0 modulo 3 (denoted as `state 0`).
2. The current sum is 1 modulo 3 (denoted as `state 1`).
3. The current sum is 2 modulo 3 (denoted as `state 2`).

We start at `state 0` (since 0 is the default sum before we look at any digits). As we see each digit, we transition between states. The goal is to end in `state 0` because that means the number is divisible by 3.

#### How Digits Change the State

Here's how each digit impacts the state:

- **Digits: `0`, `3`, `6`, and `9`**
  
  These digits are themselves multiples of 3. Adding them doesn't change the current sum modulo 3. They keep the state unchanged. So, if we are in `state 0` and see a `3`, we remain in `state 0`.
  
  **Example:**
  - You're in State 0 and you encounter a `3`. `0 + 3 = 3` which is 0 modulo 3, so you stay in State 0. 
  - You're in State 1 (meaning you're 1 modulo 3) and you see a `6`. `1 + 6 = 7` which is 1 modulo 3, so you stay in State 1.

- **Digits: `1` and `4`**
  
  These digits add 1 to the current sum. So, they transition the state as follows:
  - State 0 to State 1
  - State 1 to State 2
  - State 2 to State 0
  
  **Example:**
  - You're in State 0 and you encounter a `1`. `0 + 1 = 1` which is 1 modulo 3, so you move to State 1. 
  - You're in State 1 and you see a `4`. `1 + 4 = 5` which is 2 modulo 3, so you move to State 2.

- **Digits: `2`, `5`, and `8`**
  
  These digits add 2 to the current sum. They transition states as:
  - State 0 to State 2
  - State 2 to State 1
  - State 1 to State 0
  
  **Example:**
  - You're in State 0 and you encounter a `2`. `0 + 2 = 2` which is 2 modulo 3, so you move to State 2. 
  - You're in State 2 and you see an `8`. `2 + 8 = 10` which is 1 modulo 3 (because 10 divided by 3 has a remainder of 1), so you move to State 1.

##### Practical Example

Let's look at the number `218`.

Start at State 0:

1. See `2`. This takes you to State 2 (0 + 2 = 2, which is 2 modulo 3).
2. See `1`. This takes you to State 0 (2 + 1 = 3, which is 0 modulo 3 because it's a multiple of 3).
3. See `8`. This takes you to State 2 again (0 + 8 = 8, which is 2 modulo 3).

At the end of processing `218`, we're in State 2, so `218` is NOT divisible by 3.

### Regex for State Transitions

To capture the above logic in regex:

1. `[0369]`: This is a digit that doesn't change the state.
2. `[147][0369]*[258]`: This captures a transition from `state 0` to `state 1` and then to `state 2`.
3. `[258][0369]*[147]`: This captures a transition from `state 0` to `state 2` and then to `state 1`.

Using these patterns, the regex tries to start in `state 0`, process digits (transitioning between states as necessary), and end in `state 0`.

#### Constructing the Full Regex

With the patterns above, we can construct the full regex to account for all possible digit sequences that will land us back in `state 0` (making the number divisible by 3). That's the monstrous regex I provided.

The regex is attempting to encapsulate the logic of the state transitions we described. But note, this regex is more of a theoretical approach to show it's possible. In a practical scenario, using regex to determine divisibility by 3 isn't very efficient or readable.

It's much easier to just sum the digits and check if the sum is divisible by 3 using conventional programming.