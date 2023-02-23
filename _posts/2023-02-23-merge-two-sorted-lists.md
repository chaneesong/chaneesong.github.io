---
title:  Merge Two Sorted Lists
author: chaneesong
date: 2023-02-23 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

## 문제 설명

정렬 된 두 개의 연결 리스트 `list1`과 `list2`의 `head`가 주어진다.

두 리스트를 하나로 병합하라. 이 리스트는 두 리스트의 노드를 이어 붙여서 만들어야 한다.

병합된 연결 리스트의 head를 반환하라.

## 해결 방법

재귀를 활용하여, 두 리스트를 병합한다.  
물론 반복문을 통해서 똑같이 해결할 수 있지만 재귀에 비해 반복문은 코드가 더 길어지는 특성이 있다.  
하지만 재귀는 반복문에 비해 코드의 흐름을 읽기가 쉽지 않고, 대부분의 상황에서 반복문보다 메모리를 더 많이 사용하므로 상황에 따라 적절히 사용하는 것이 중요하다.

1. 재귀의 종료 조건은 list1 또는 list2가 없을 때 종료된다.
2. list1 < list2일 때 list1을 한 칸 뒤로 옮기고 재귀 호출을 진행한다.
3. list1 > list2일 때 list2를 한 칸 뒤로 옮기고 재귀 호출을 진행한다.

재귀는 다음과 같은 방식으로 연결된다.

![movement](/assets/img/algorithm/linked-list/merge-two-sorted-lists/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
// Definition for singly-linked list.
export class ListNode {
  val: number;
  next: ListNode | null;
  constructor(val?: number, next?: ListNode | null) {
    this.val = val === undefined ? 0 : val;
    this.next = next === undefined ? null : next;
  }
}

export function mergeTwoLists(
  list1: ListNode | null,
  list2: ListNode | null
): ListNode | null {
  if (!list1) return list2;
  if (!list2) return list1;

  if (list1.val < list2.val) {
    list1.next = mergeTwoLists(list1.next, list2);
    return list1;
  }
  list2.next = mergeTwoLists(list1, list2.next);
  return list2;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { mergeTwoLists, ListNode } from '.';

const convertArrayToList = (array: number[]): ListNode | null => {
  if (!array) return null;

  const head = new ListNode(-1);
  let prev = head;

  for (let i = 0; i < array.length; i++) {
    prev.next = new ListNode(array[i]);
    prev = prev.next;
  }

  return head.next;
};

describe(' description', () => {
  test('example test 1', () => {
    const list1 = convertArrayToList([1, 2, 4]);
    const list2 = convertArrayToList([1, 3, 4]);
    const output = mergeTwoLists(list1, list2);
    const expected = convertArrayToList([1, 1, 2, 3, 4, 4]);
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const list1 = null;
    const list2 = null;
    const output = mergeTwoLists(list1, list2);
    const expected = null;
    expect(output).toEqual(expected);
  });

  test('example test 3', () => {
    const list1 = null;
    const list2 = convertArrayToList([0]);
    const output = mergeTwoLists(list1, list2);
    const expected = new ListNode(0);
    expect(output).toEqual(expected);
  });
});
```

## 구현 후

재귀를 막상 GIF로 만드려니 너무 손이 많이간다. 재귀를 그림으로 도식화 할 때는 파이썬 튜터를 한 번 활용해보는 것도 나쁘지 않겠다.
