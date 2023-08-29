---
title: "Animal Shelter"
categories:
  - dsa
  - problem
tags:
  - queue
  - linked list
toc: true
# header:
#   image: /assets/images/dsa/00113-2615830643.jpg
---

## PROBLEM

An animal shelter, which holds only dogs and cats, operates on a strictly "first in, first out" basis. People must adopt either the "oldest" (based on arrival time) of all animals at the shelter, or they can select whether they would prefer a dog or a cat (and will receive the oldest animal of that type). They cannot select which specific animal they would like. Create the data structures to maintain this system and implement operations such as `enqueue`, `dequeueAny`, `dequeueDog`, and `dequeueCat`. You may use the built-in `LinkedList` data structure.

### HINTS

**#22** We could consider keeping a single linked list for dogs and cats, and then iterating through it to find the first dog (or cat). What is the impact of doing this?
{: .notice}

**#56** Let's suppose we kept separate lists for dogs and cats. How would we find the oldest animal of any type? Be creative!
{: .notice--warning}

**#63** Think about how you'd do it in real life. You have a list of dogs in chronological order and a list of cats in chronological order. What data would you need to find the oldest animal? How would you maintain this data?
{: .notice--success}

## SOLUTION

To implement this system, we could maintain two separate linked lists: one for dogs and one for cats. Each node in the linked list would contain not just the animal but also a timestamp indicating when the animal was enqueued. This timestamp could simply be an increasing integer value, incremented each time any animal (cat or dog) is enqueued.

To find the oldest animal of any type, we would compare the timestamps of the front nodes of the two linked lists and dequeue from the list with the oldest timestamp.

Let's proceed with the solution:

1. Maintain two linked lists, one for dogs and one for cats.
2. Each node in the linked lists will contain an animal and a timestamp.
3. Maintain a global timestamp that increments on each enqueue operation.

Here's a possible implementation in TypeScript:

```typescript
class AnimalNode {
  animal: string;
  timestamp: number;
  next: AnimalNode | null = null;

  constructor(animal: string, timestamp: number) {
    this.animal = animal;
    this.timestamp = timestamp;
  }
}

class AnimalShelter {
  private dogList: AnimalNode | null = null;
  private catList: AnimalNode | null = null;
  private timestamp: number = 0;

  enqueue(animal: string): void {
    const newNode = new AnimalNode(animal, this.timestamp);
    this.timestamp++;

    if (animal === "dog") {
      if (!this.dogList) {
        this.dogList = newNode;
      } else {
        let current = this.dogList;
        while (current.next) {
          current = current.next;
        }
        current.next = newNode;
      }
    } else {
      if (!this.catList) {
        this.catList = newNode;
      } else {
        let current = this.catList;
        while (current.next) {
          current = current.next;
        }
        current.next = newNode;
      }
    }
  }

  dequeueAny(): string | null {
    if (!this.dogList && !this.catList) return null;

    if (!this.dogList) return this.dequeueCat();
    if (!this.catList) return this.dequeueDog();

    if (this.dogList.timestamp < this.catList.timestamp) {
      return this.dequeueDog();
    } else {
      return this.dequeueCat();
    }
  }

  dequeueDog(): string | null {
    if (!this.dogList) return null;

    const oldestDog = this.dogList.animal;
    this.dogList = this.dogList.next;
    return oldestDog;
  }

  dequeueCat(): string | null {
    if (!this.catList) return null;

    const oldestCat = this.catList.animal;
    this.catList = this.catList.next;
    return oldestCat;
  }
}

// Example usage:
const shelter = new AnimalShelter();
shelter.enqueue('dog');
shelter.enqueue('cat');
shelter.enqueue('dog');

console.log(shelter.dequeueAny());  // "dog"
console.log(shelter.dequeueCat());  // "cat"
console.log(shelter.dequeueDog());  // "dog"
```

In this implementation:

- `enqueue` adds an animal to the appropriate list, and updates its timestamp.
- `dequeueAny` checks the timestamps of the front of both lists and dequeues the oldest animal.
- `dequeueDog` and `dequeueCat` remove and return the oldest dog or cat, respectively.
