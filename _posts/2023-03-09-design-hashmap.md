---
title: Design HashMap
author: chaneesong
date: 2023-03-09 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [706. Design HashMap](https://leetcode.com/problems/design-hashmap/description/)

## 문제 설명

내장 된 해시 테이블 라이브러리를 사용하지 않고 해시맵을 디자인하세요.

`MyHashMap` class 설계도:

- `MyHashMap()` 비어있는 맵 객체 초기화
- `void put(int key, int value)` 해시맵에 `(key, value)`쌍을 삽입. 맵에 키가 이미 존재하는 경우 해당 `value`를 업데이트 한다.
- `int get(int key)` 지정된 `key`에 매핑된 `value`를 리턴. `key`가 없으면 `-1`
- `void remove(int key)` 맵에 `key`가 매핑된 경우 해당 `key`와 `value`를 삭제

## 해결 방법

해시 테이블을 배열로 만들고, 해시 충돌의 경우 `linked list`로 체이닝하여 해결한다.
hashing은 `key % Array length`를 하여 간단하게 해싱한다.

- `void put(int key, int value)` 해싱한 위치가 비어있으면 만들어서 넣고, 비어있지 않으면 리스트를 탐색하여 `key` 값이 존재한다면 `value` 업데이트, 없으면 마지막에 연결한다.
- `int get(int key)` 해싱한 위치에 리스트를 탐색하면서 일치하는 `key`가 있으면 `value`리턴
- `void remove(int key)` 해싱한 위치의 리스트를 탐색하면서 값이 존재하면 해당 노드의 앞과 뒤를 연결시킴

### Notes

삭제의 경우에 첫 번째 노드가 일치한다면, 변수의 위치를 변경시키는 것으로는 실제 위치가 변경되지 않는다. 그렇기 때문에 실제 위치인 `this.table[index]`의 노드를 직접 변경시켜야 한다.

## 풀이 코드

```typescript
class MyListNode {
  key: number;
  value: number;
  next: MyListNode | null;
  constructor(key: number, value: number, next?: MyListNode) {
    this.key = key;
    this.value = value;
    this.next = next ? next : null;
  }
}

class MyHashMap {
  private size: number;
  private table: Array<MyListNode | null>;
  constructor() {
    this.size = 5;
    this.table = new Array(this.size);
  }

  put(key: number, value: number): void {
    const index = key % this.size;

    if (!this.table[index]) {
      this.table[index] = new MyListNode(key, value);
      return;
    }

    let node = this.table[index];
    while (node.next !== null && node.key !== key) {
      node = node.next;
    }

    if (node.key === key) {
      node.value = value;
      return;
    }
    node.next = new MyListNode(key, value);
  }

  get(key: number): number {
    const index = key % this.size;

    if (!this.table[index]) return -1;

    let node = this.table[index];
    while (node) {
      if (node.key === key) return node.value;
      node = node.next;
    }
    return -1;
  }

  remove(key: number): void {
    const index = key % this.size;
    if (!this.table[index]) return;

    let node = this.table[index];
    if (node.key === key) {
      this.table[index] = node.next;
      return;
    }

    let prev = node;
    while (node) {
      if (node.key === key) {
        prev.next = node.next;
        return;
      }
      prev = node;
      node = node.next;
    }
  }
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  let myHashMap: MyHashMap;
  beforeAll(() => {
    myHashMap = new MyHashMap();
  });

  afterAll(() => {
    myHashMap = null;
  });

  test('put [ 1: 1 ] in hashmap', () => {
    myHashMap.put(1, 1);
    expect(myHashMap.get(1)).toBe(1);
  });
  test('put [ 2: 2 ] in hashmap', () => {
    myHashMap.put(6, 6);
    expect(myHashMap.get(6)).toBe(6);
  });
  test('get key: 1 in hashmap', () => {
    expect(myHashMap.get(1)).toBe(1);
  });
  test('get key: 3 in hashmap', () => {
    expect(myHashMap.get(3)).toBe(-1);
  });
  test('put [ 2: 1 ] in hashmap', () => {
    myHashMap.put(2, 1);
    expect(myHashMap.get(2)).toBe(1);
  });
  test('get key: 2 in hashmap', () => {
    expect(myHashMap.get(2)).toBe(1);
  });
  test('remove key: 2 in hashmap', () => {
    myHashMap.remove(6);
    expect(myHashMap.get(6)).toBe(-1);
  });
  test('get key: 2 in hashmap', () => {
    expect(myHashMap.get(6)).toBe(-1);
  });
});
```
