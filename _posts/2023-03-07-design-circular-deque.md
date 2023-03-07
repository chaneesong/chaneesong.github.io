---
title: Design Circular Deque
author: chaneesong
date: 2023-03-07 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [641. Design Circular Deque](https://leetcode.com/problems/design-circular-deque/description/)

## 문제 설명

원형 데크를 구현하는 방법에 대해 설계해보세요.

- `MyCircularDeque(int k)` 최대 크기가 k인 데크를 초기화
- `boolean insertFront()` 데크의 첫 번째에 아이템을 삽입한다. 성공이면 true, 아니면 false
- `boolean insertLast()` 데크의 마지막에 아이템을 삽입한다. 성공이면 true, 아니면 false
- `boolean deleteFront()` 데크의 첫 번째 아이템을 삭제한다. 성공이면 true, 아니면 false
- `boolean deleteLast()` 데크의 마지막 아이템을 삭제한다. 성공이면 true, 아니면 false
- `int getFront()` 데크의 첫 번째 아이템을 반환한다. 없으면 -1
- `int getRear()` 데크의 마지막 아이템을 반환한다. 없으면 -1
- `boolean isEmpty()` 데크가 비어있으면 true, 아니면 false
- `boolean isFull()` 데크가 가득 찼으면 true, 아니면 false

## 해결 방법

원형 데크는 연결 리스트와 배열로 구현 할 수 있는데, 원형 데크의 이점을 잘 살리기 위해 배열로 구현한다.

### 설계

1. 원형 데크는 앞 뒤로 삽입 및 삭제가 가능하기 때문에 포인터 두개(`head`, `tail`)를 활용한다.
2. 두 포인터의 위치가 같을 때는 삽입 및 삭제 시 포인터를 이동하지 않는다.
3. 데크에 삽입 할 경우 가득 찼는지 체크를 한다.
   - 데크 앞 쪽에 삽입 시 `head`를 한 칸 앞으로 이동 후 삽입한다.
   - 데크 뒤 쪽에 삽입 시 `tail`를 한 칸 뒤로 이동 후 삽입한다.
4. 데크에서 삭제할 경우 비어있는지 체크한다.
   - 데크 앞 쪽에서 삭제 시 삭제 후 `head`를 한 칸 뒤로 이동한다.
   - 데크 뒤 쪽에서 삭제 시 삭제 후 `tail`을 한 칸 앞으로 이동한다.
5. `head == tail`이고, `head` 위치의 아이템이 없으면 비어있는 것으로 판단한다.
6. `tail`의 다음 위치에 값이 있으면 가득 찬 것으로 판단한다.

## 풀이 코드

```typescript
class MyCircularDeque {
  private deque: Array<number>;
  private head: number;
  private tail: number;
  private maxLen: number;

  constructor(k: number) {
    this.deque = new Array(k).fill(undefined);
    this.head = 0;
    this.tail = 0;
    this.maxLen = k;
  }

  insertFront(value: number): boolean {
    if (this.isFull()) return false;
    if (!this.isEmpty()) {
      this.head = (this.head - 1 + this.maxLen) % this.maxLen;
    }
    this.deque[this.head] = value;

    return true;
  }

  insertLast(value: number): boolean {
    if (this.isFull()) return false;
    if (!this.isEmpty()) {
      this.tail = (this.tail + 1) % this.maxLen;
    }
    this.deque[this.tail] = value;

    return true;
  }

  deleteFront(): boolean {
    if (this.isEmpty()) return false;
    this.deque[this.head] = undefined;
    if (this.head !== this.tail) {
      this.head = (this.head + 1) % this.maxLen;
    }
    return true;
  }

  deleteLast(): boolean {
    if (this.isEmpty()) return false;
    this.deque[this.tail] = undefined;
    if (this.head !== this.tail) {
      this.tail = (this.tail - 1 + this.maxLen) % this.maxLen;
    }
    return true;
  }

  getFront(): number {
    return this.isEmpty() ? -1 : this.deque[this.head];
  }

  getRear(): number {
    return this.isEmpty() ? -1 : this.deque[this.tail];
  }

  isEmpty(): boolean {
    return this.head === this.tail && this.deque[this.head] === undefined;
  }

  isFull(): boolean {
    const nextIndex = (this.tail + 1) % this.maxLen;
    return this.deque[nextIndex] !== undefined;
  }
}
```

## 테스트 코드

```typescript
import { MyCircularDeque } from '.';

describe('circular deque test suit', () => {
  let circularDeque: MyCircularDeque;
  beforeAll(() => {
    circularDeque = new MyCircularDeque(3);
  });

  afterAll(() => {
    circularDeque = undefined;
  });
  test('insertLast', () => {
    expect(circularDeque.insertLast(1)).toBe(true);
    expect(circularDeque.getFront()).toBe(1);
    expect(circularDeque.getRear()).toBe(1);
  });
  test('insertLast', () => {
    expect(circularDeque.insertLast(2)).toBe(true);
    expect(circularDeque.getFront()).toBe(1);
    expect(circularDeque.getRear()).toBe(2);
  });
  test('insertFront', () => {
    expect(circularDeque.insertFront(3)).toBe(true);
    expect(circularDeque.getFront()).toBe(3);
    expect(circularDeque.getRear()).toBe(2);
  });
  test('insertFront', () => {
    expect(circularDeque.insertFront(4)).toBe(false);
    expect(circularDeque.getFront()).toBe(3);
    expect(circularDeque.getRear()).toBe(2);
  });
  test('getRear', () => {
    expect(circularDeque.getRear()).toBe(2);
  });
  test('deleteLast', () => {
    expect(circularDeque.deleteLast()).toBe(true);
    expect(circularDeque.getFront()).toBe(3);
    expect(circularDeque.getRear()).toBe(1);
  });
  test('insertFront', () => {
    expect(circularDeque.insertFront(4)).toBe(true);
    expect(circularDeque.getFront()).toBe(4);
    expect(circularDeque.getRear()).toBe(1);
  });
  test('getFront', () => {
    expect(circularDeque.getFront()).toBe(4);
  });
});
```

## 구현 후

원래는 이중 연결 리스트로 구현하려 했지만, 이중 연결 리스트의 경우 리스트를 초기화하는 작업에서 구현 난이도가 증가하기 때문에  간단하게 배열로 구현했다.
