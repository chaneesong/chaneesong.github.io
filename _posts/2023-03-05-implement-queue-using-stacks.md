---
title: Implement Queue using Stacks
author: chaneesong
date: 2023-03-05 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/description/)

## 문제 설명

두 개의 스택을 이용하여 FIFO 구조의 큐를 구현하라. 구현된 큐는 큐의 기능(`push`, `peek`, `pop`, `empty`)을 지원해야한다.

`MyQueue` 클래스 구현:

- `void push(int x)` 큐의 가장 마지막에 요소 x 삽입
- `int pop()` 큐의 가장 앞 요소를 삭제 후 반환
- `int peek()` 큐의 가장 앞 요소를 반환
- `boolean empty()` 큐가 비어있으면 `true` 반환, 아니면 `false`

### Notes

- 스택의 표준 기능만 사용해야한다. 즉, `push on top`, `peek/pop`, `size`, `is empty` 기능만 사용가능하다.
- 언어에 따라서 스택을 지원하지 않을 수도 있다. 스택의 표준 연산만 사용한다면 리스트나 데크로 스택을 구현해도 된다.

## 해결 방법

리스트 두 개(`input stack`, `output stack`)를 사용하여 큐를 구현한다.

- `void push(int x)`: `input`에 요소 x를 `push`한다.
- `int peek()`: `output`에 요소가 있는지 확인하고, 없다면 `input`에서 꺼내 `ouput`에 넣는다. 그 후 `output`의 마지막 요소를 반환한다.
- `int pop()`: `push`에서 정렬을 진행하지 않기 때문에 `output`에서 꺼내기 전 정렬을 진행해야한다. `peek`을 사용하면 정렬이 되기 때문에 `peek`을 이용해 정렬 후 `output`에서 `pop`한다.
- `boolean empty()`: `input`과 `output`이 모두 비어있으면 `true`, 아니면 `false`

## 풀이 코드

```typescript
class MyQueue {
  private input: number[];
  private output: number[];
  constructor() {
    this.input = [];
    this.output = [];
  }

  push(x: number): void {
    this.input.push(x);
  }

  pop(): number {
    this.peek();
    return this.output.pop();
  }

  peek(): number {
    if (!this.output.length) {
      while (this.input.length) {
        this.output.push(this.input.pop());
      }
    }
    return this.output.at(-1);
  }

  empty(): boolean {
    return this.input.length || this.output.length ? false : true;
  }
}
```

## 테스트 코드

```typescript
import { MyQueue } from '.';

describe(' description', () => {
  let queue: MyQueue;
  beforeAll(() => {
    queue = new MyQueue();
  });

  afterAll(() => {
    queue = null;
  });
  test('push', () => {
    queue.push(1);
    expect(queue.peek()).toBe(1);
  });
  test('push', () => {
    queue.push(2);
    expect(queue.peek()).toBe(1);
  });
  test('peek', () => {
    expect(queue.peek()).toBe(1);
  });
  test('pop', () => {
    expect(queue.pop()).toBe(1);
  });
  test('empty', () => {
    expect(queue.empty()).toBe(false);
  });
});
```
