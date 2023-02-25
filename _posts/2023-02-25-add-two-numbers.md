---
title: Add Two Numbers
author: chaneesong
date: 2023-02-25 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/description/)

## 문제 설명

자연수로 나타내고 비어있지 않은 두 개의 연결 리스트가 주어진다. 숫자는 역순으로 저장되어 있고, 각각의 노드에는 한 자리 숫자가 들어있다.  
두 개의 숫자를 더한 연결 리스트를 반환하라.

두 숫자에는 필요한 0을 제외하고, 선행하는 0이 포함되어 있지 않다고 가정한다(가장 앞에 0은 없다).

## 해결 방법

역순으로 연결되어 있기 때문에 자연스럽게 우리가 손으로 계산하는 것처럼 하면 된다.

1. 올림 값을 더한다.
2. l1이 있으면 더한다.
3. l2가 있으면 더한다.
4. 새로운 리스트를 만들어 `더한 값 % 10`을 연결한다.
5. `더한 값 / 10`의 몫을 올림 값에 저장한다.
6. 끝까지 반복한다.

## 풀이 코드

```typescript
function addTwoNumbers(l1: ListNode | null, l2: ListNode | null): ListNode | null {
  const head = new ListNode(-1);
  let cur = head;
  let up = 0;

  while (l1 || l2 || up) {
    let sum = up;

    if (l1) {
      sum += l1.val;
      l1 = l1.next;
    }
    if (l2) {
      sum += l2.val;
      l2 = l2.next;
    }

    cur.next = new ListNode(sum % 10);
    up = Math.floor(sum / 10);
    cur = cur.next;
  }

  return head.next;
}

```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { addTwoNumbers } from '.';
import { convertArrayToList } from '../utils';

describe(' description', () => {
  test('example test 1', () => {
    const l1 = convertArrayToList([2, 4, 3]);
    const l2 = convertArrayToList([5, 6, 4]);
    const output = addTwoNumbers(l1, l2);
    const expected = convertArrayToList([7, 0, 8]);
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const l1 = convertArrayToList([0]);
    const l2 = convertArrayToList([0]);
    const output = addTwoNumbers(l1, l2);
    const expected = convertArrayToList([0]);
    expect(output).toEqual(expected);
  });

  test('example test 3', () => {
    const l1 = convertArrayToList([9, 9, 9, 9, 9, 9, 9]);
    const l2 = convertArrayToList([9, 9, 9, 9]);
    const output = addTwoNumbers(l1, l2);
    const expected = convertArrayToList([8, 9, 9, 9, 0, 0, 0, 1]);
    expect(output).toEqual(expected);
  });
});

```
