---
title: Reverse Linked List
author: chaneesong
date: 2023-02-24 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

## 문제 설명

단일 연결 리스트의 `head`가 주어질 때, 리스트를 뒤집고, 뒤집어진 리스트를 리턴하라.

## 해결 방법

재귀를 이용하여 리스트를 뒤집는다.

1. 재귀의 종료 조건 받은 `node`인자가 `null`일 때 종료한다(리스트를 끝까지 순회 했을 때).
2. 현재 리스트의 다음 노드를 저장하고, 이전 노드를 다음 노드로 연결한다.
3. 재귀 탐색이 종료 될 때까지 반복한다.

![movement](/assets/img/algorithm/linked-list/reverse-linked-list/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
import { ListNode } from '../utils';

export function reverseList(head: ListNode | null): ListNode | null {
  if (!head) return head;
  const reverse = (node: ListNode, prev: ListNode | null = null): ListNode => {
    if (!node) return prev;

    const next = node.next;
    node.next = prev;

    return reverse(next, node);
  };

  return reverse(head);
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { reverseList } from '.';
import { convertArrayToList } from '../utils';

describe(' description', () => {
  test('example test 1', () => {
    const input = convertArrayToList([1, 2, 3, 4, 5]);
    const output = reverseList(input);
    const expected = convertArrayToList([5, 4, 3, 2, 1]);
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const input = convertArrayToList([1, 2]);
    const output = reverseList(input);
    const expected = convertArrayToList([2, 1]);
    expect(output).toEqual(expected);
  });

  test('example test 3', () => {
    const input = convertArrayToList([]);
    const output = reverseList(input);
    const expected = convertArrayToList([]);
    expect(output).toEqual(expected);
  });
});
```

## 구현 후

해당 문제에서 재귀를 이미지로 만들 때 `next` 포인터를 넣을 지 말지 고민했다.  
`next` 포인터를 넣지 않으면 흐름 상 `node.next`와의 연결이 끊기기 때문에 리스트를 탐색할 수 없다.  
`next`를 넣지 않아도 동작을 이해하는데 크게 문제가 없었으므로 이미지로 만들 때는 `next`포인터를 제외하고 만들었다.  
