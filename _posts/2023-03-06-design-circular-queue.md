---
title: Design Circular Queue
author: chaneesong
date: 2023-03-06 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/description/)

## 문제 설명

원형 큐의 구현을 디자인하라. 원형 큐는 FIFO 원리에 따라 수행하는 선형 자료구조이며, 마지막이 첫 번째와 연결되어 원형을 이룬다. 이 때문에 "Ring Buffer"라고도 부른다.

원형 큐의 한 가지 이점은 큐의 앞 공간을 활용할 수 있다는 것이다. 보통의 큐는 한 번 가득 찬 후, 앞의 공간에는 더 이상 다음 값을 넣을 수 없다. 하지만 원형 큐를 사용하게 되면, 해당 공간에 새로운 값을 저장 할 수 있다.

`MyCircularQueue` 클래스 구현:

- `MyCircularQueue(k)`: 큐의 사이즈를 `k`로 초기화
- `int Front()`: 큐의 첫 번째 값을 가져온다. 비어있으면 -1 반환
- `int Rear()`: 큐의 마지막 값을 가져온다. 비어있으면 -1 반환
- `boolean enQueue(int value)`: 원형큐에 요소 삽입. 성공적으로 동작했으면 true 반환
- `boolean deQueue()` 큐의 요소를 삭제한다. 성공적으로 동작했으면 true 반환
- `boolean isEmpty()` 큐가 비어있는지 체크
- `boolean isFull()` 큐가 가득 찼는지 체크

프로그래밍 언어에 내장 된 큐 자료구조를 사용하지 않고 문제를 해결해야 한다.

## 해결 방법

리스트와 포인터 두 개를 가지고 해결한다.

- `boolean isEmpty()`: 포인터 두 개의 위치가 같고, 해당 위치의 값이 `undefined`일 때 `true`
- `boolean isFull()`: 포인터 두 개의 위치가 같고, 해당 위치의 값이 존재 할 때 `true`
- `int Front()`: `p1`의 위치에 값이 존재 하면 `value` 리턴
- `int Rear()`: `p2`의 위치에 값이 존재 하면 `value` 리턴
- `boolean enQueue(int value)`: 공간이 있으면 값을 삽입 후 p2 포인터 한 칸 뒤로 이동
- `boolean deQueue()`: 꺼낼 수 있는 값이 있으면 삭제 후 p1 포인터 한 칸 뒤로 이동

### Notes

- 원형 큐에서는 리스트의 마지막 위치에서 한 칸 뒤로 이동하게 된다면 첫 번째 위치가 되므로 나머지 연산을 활용해 문제를 해결한다.
- 또한, 마지막을 가리키는 포인터가 `마지막 값 + 1`일 경우 마지막 위치를 찾을 때 `p2 -1`을 진행하는데 예외로 `0 - 1 = -1`이기 때문에 따로 처리가 필요하다.
- 항상 값이 있는 지 비교 할 때에는 falsy 값이 무엇인지 다시 한 번 확인한다.

## 풀이 코드

```typescript
class MyCircularQueue {
  private queue: Array<number>;
  private maxLen: number;
  private p1 = 0;
  private p2 = 0;

  constructor(k: number) {
    this.queue = new Array<number>(k).fill(undefined);
    this.maxLen = k;
  }

  enQueue(value: number): boolean {
    if (this.isFull()) return false;
    this.queue[this.p2] = value;
    this.p2 = (this.p2 + 1) % this.maxLen;
    return true;
  }

  deQueue(): boolean {
    if (this.isEmpty()) return false;
    this.queue[this.p1] = undefined;
    this.p1 = (this.p1 + 1) % this.maxLen;
    return true;
  }

  Front(): number {
    return this.queue[this.p1] !== undefined ? this.queue[this.p1] : -1;
  }

  Rear(): number {
    return this.queue.at(this.p2 - 1) !== undefined
      ? this.queue.at(this.p2 - 1)
      : -1;
  }

  isEmpty(): boolean {
    return this.p1 === this.p2 && this.queue[this.p1] === undefined;
  }

  isFull(): boolean {
    return this.p1 === this.p2 && this.queue[this.p1] !== undefined;
  }
}
```

## 테스트 코드

```typescript
import { MyCircularQueue } from '.';

describe('example test suit', () => {
  let circularq: MyCircularQueue;
  beforeAll(() => {
    circularq = new MyCircularQueue(3);
  });

  afterAll(() => {
    circularq = undefined;
  });

  test('enQueue 1', () => {
    expect(circularq.Front()).toBe(-1);
    expect(circularq.enQueue(1)).toBe(true);
  });
  test('enQueue 2', () => {
    expect(circularq.Front()).toBe(1);
    expect(circularq.enQueue(2)).toBe(true);
  });
  test('enQueue 3', () => {
    expect(circularq.Front()).toBe(1);
    expect(circularq.enQueue(3)).toBe(true);
  });
  test('enQueue 4', () => {
    expect(circularq.Front()).toBe(1);
    expect(circularq.enQueue(4)).toBe(false);
  });
  test('Rear', () => {
    expect(circularq.Rear()).toBe(3);
  });
  test('isFull', () => {
    expect(circularq.isFull()).toBe(true);
  });
  test('deQueue', () => {
    expect(circularq.deQueue()).toBe(true);
  });
  test('enQueue 4', () => {
    expect(circularq.Front()).toBe(2);
    expect(circularq.enQueue(4)).toBe(true);
  });
  test('Rear', () => {
    expect(circularq.Rear()).toBe(4);
  });
});
```
