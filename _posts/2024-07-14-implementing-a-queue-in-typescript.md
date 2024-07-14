---
layout: post
title: Implementing a Queue in TypeScript
date: 2024-07-14 00:00:00 -0500
description: Implementing a queue in TypeScript
categories: TypeScript data-structures
author: Angel Baez
---

## What is a queue?

A queue is a data structure that stores elements using the FIFO (First In First Out) strategy for accessing methods.

Think of a queue as a line of people waiting for a service, like at a bank or a movie theater. The first person in line is the first one to be served (First-In-First-Out, or FIFO). This simple concept has numerous applications in computing.

## Why is a queue important?

Understanding queue helps you grasp more complex data structures and algorithms. Also, learning queues enhances your programming skills. Implementing a queue from zero gives you a deeper understanding of memory management and data organization.

## How to implement a queue in TypeScript

Here is a simple implementation of a queue in TypeScript:

```typescript
type Node<T> = {
  value: T;
  next?: Node<T>;
};

export default class Queue<T> {
  public length: number;
  private head?: Node<T>;
  private tail?: Node<T>;

  constructor() {
    this.head = this.tail = undefined;
    this.lenght = 0;
  }
  enqueue(item: T): void {
    const node = { value: item } as Node<T>;
    this.length++;
    if (!this.tail) {
      this.tail = this.head = node;
      return;
    }
    this.tail.next = node;
    this.tail = node;
  }
  deque(): T | undefined {
    if (!this.head) {
      return undefined;
    }
    this.length--;

    const head = this.head;
    this.head = this.head.next;

    head.next = undefined;

    if (this.length === 0) {
      this.tail = undefined;
    }

    return head.value;
  }
  peek(): T | undefined {
    return this.head?.value;
  }
}
```
