---
title: Implement Stack using Queues
author: chaneesong
date: 2023-03-04 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/description/)

## 문제 설명

후입선출(LIFO)구조인 stack을 오직 2개의 큐만 활용하여 구현하라. 구현된 스택은 보통의 스택 기능(`push`, `top`, `pop`, `empty`)을 지원해야한다.

MyStack class 구현:

- `void push(int x)` 요소 x를 스택의 가장 위에 삽입
- `int pop()` 스택의 top을 제거 후, 반환
- `int top()` 스택의 가장 위의 요소를 반환
- `boolean empty()` 스택이 비어있으면 `true` 리턴, 아니면 `false`

## 해결 방법

정석적인 풀이라면 큐를 두 개 활용해야겠지만, 자바스크립트에서는 큐를 하나만 사용해서 구현할 수 있다.

- `void push(int x)`: 큐에 요소를 삽입(enqueue)한다. 그 후, 마지막 요소를 제외한 나머지 요소를 추출(dequeue)하여 다시 삽입한다.
- `int pop()`: 큐에서 요소를 추출(enqueue)한다.
- `int top()`: 큐의 첫 번째 요소를 반환한다.
- `boolean empty()`: 큐의 길이를 0인지 확인하여 리턴한다.

## 풀이 코드

```typescript
export class MyStack {
  private queue: number[];

  constructor() {
    this.queue = [];
  }

  push(x: number): void {
    this.queue.push(x);
    for (let i = 0; i < this.queue.length - 1; i++) {
      this.queue.push(this.queue.shift());
    }
  }

  pop(): number {
    return this.queue.shift();
  }

  top(): number {
    return this.queue[0];
  }

  empty(): boolean {
    return this.queue.length ? false : true;
  }
}
```

## 테스트 코드

```typescript
import { MyStack } from '.';

describe(' description', () => {
  let stack: MyStack;

  beforeAll(() => {
    stack = new MyStack();
  });

  afterAll(() => {
    stack = null;
  });

  test('push test in stack', () => {
    stack.push(1);
    expect(stack.top()).toBe(1);
  });

  test('push test in stack', () => {
    stack.push(2);
    expect(stack.top()).toBe(2);
  });

  test('top test in stack', () => {
    const top = stack.top();
    expect(top).toBe(2);
  });

  test('pop test in stack', () => {
    const data = stack.pop();
    expect(data).toBe(2);
  });

  test('empty test in stack', () => {
    const isEmpty = stack.empty();
    expect(isEmpty).toBe(false);
  });
});
```

## 구현 후

테스트코드를 작성할 때 beforeAll을 사용하여 테스트 과정 중 공유되는 데이터를 활용할 수 있다. 또한, 위의 stack구현에서는 push를 할 때마다 queue를 뒤집지만, 개별 요소에 대해 접근 시간이 O(1)이라면, 속도 면에서는 pop을 할 때 뒤집는게 더 나을 것이다. 하지만 그렇게 되면 또, 마지막 요소의 인덱스를 가지고 있는 변수를 할당하거나 하는 추가적인 작업이 필요하므로 간단하게 구현할 때는 push할 때 뒤집는게 편하다.
