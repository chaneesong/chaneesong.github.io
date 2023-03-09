---
title: Merge K Sorted Lists
author: chaneesong
date: 2023-03-08 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [23. Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)

## 문제 설명

k 개의 연결 리스트로 이루어진 배열 `lists`가 주어지며, 각 연결 리스트는 오름차순으로 정렬되어있다.

모든 연결 리스트를 하나의 정렬된 연결 리스트로 병합하고, 반환하라.

## 해결 방법

해당 풀이는 **아주 야매 풀이 방식**이다. 제대로 된 풀이를 하려면 힙을 구현하거나, 리스트를 하나로 병합할 수 있는 재귀 풀이 방식을 사용하는 것이다.

풀이 방식은 너무나도 쉽다. k개의 리스트들을 하나의 배열로 병합한 뒤 정렬시킨다. 그 후 하나의 리스트로 만들면 끝.  
원래는 재귀 풀이 방식을 사용했는데 최적화를 하지 않은 재귀는 너무나도 느렸다(가장 빠른 풀이의 3배 정도). 그래서 가장 빠른 풀이를 봤는데 `리스트 -> 배열 -> 리스트` 방식을 사용하는 것을 보고 한 번 해보고 싶어서 이 방식으로 풀었다.

## 풀이 코드

```typescript
import { ListNode } from '../utils';

function mergeKLists(lists: Array<ListNode | null>): ListNode | null {
  if (!lists) return null;

  const tmpArr = [];
  const head = new ListNode();
  let current = head;

  for (let i = 0; i < lists.length; i++) {
    while (lists[i]) {
      tmpArr.push(lists[i].val);
      lists[i] = lists[i].next;
    }
  }

  const sortedTmpArr = tmpArr.sort((a, b) => a - b);

  sortedTmpArr.forEach((value) => {
    current.next = new ListNode(value);
    current = current.next;
  });
  return head.next;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test', () => {
    const input = [
      convertArrayToList([1, 4, 5]),
      convertArrayToList([1, 3, 4]),
      convertArrayToList([2, 6]),
    ];
    const output = mergeKLists(input);
    const expected = convertArrayToList([1, 1, 2, 3, 4, 4, 5, 6]);
    expect(output).toEqual(expected);
  });
  test('test null lists', () => {
    const input = [];
    const output = mergeKLists(input);
    const expected = convertArrayToList([]);
    expect(output).toEqual(expected);
  });

  test('test empty list', () => {
    const input = [convertArrayToList([])];
    const output = mergeKLists(input);
    const expected = convertArrayToList([]);
    expect(output).toEqual(expected);
  });
});

```
