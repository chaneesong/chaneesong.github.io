---
title: Reverse Linked List II
author: chaneesong
date: 2023-02-28 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/description/)

## 문제 설명

연결 리스트의 `head`와 두 정수 `left`와 `right`가 주어질 때(`left <= right`), `left`부터 `right`까지 노드들을 뒤집고, 연결리스트를 리턴하라.

## 해결 방법

변하지 않는 `start(left의 한칸 앞)`와 `end(left 노드)` 노드를 두고, `start`와 `end` 사이로 범위에 있는 노드들을 하나 씩 넣는다.

1. 먼저 `left`가 0이 아닌 1부터 시작하기 때문에 `root`노드를 하나 만들어서 `head` 앞에 연결한다.
2. `left - 1`에 `start` 노드를 위치시키고, `start.next`에 `end`를 위치시킨다.
3. `end.next`를 임시 변수(`tmp`)에 할당하고, `end.next`를 `tmp.next`로 변경
4. `tmp.next`를 `start.next`로 변경
5. `start.next`를 `tmp`로 변경
6. 위 과정을 끝까지 반복한다.

![movement](/assets/img/algorithm/linked-list/reverse-linked-list-ii/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function reverseBetween(
  head: ListNode | null,
  left: number,
  right: number
): ListNode | null {
  if (!head || left === right) return head;

  const root = new ListNode(0, head);
  let start = root;
  for (let i = 0; i < left - 1; i++) {
    start = start.next;
  }
  const end = start.next;

  for (let i = 0; i < right - left; i++) {
    const tmp = end.next;
    end.next = tmp.next;
    tmp.next = start.next;
    start.next = tmp;
  }

  return root.next;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const input = convertArrayToList([1, 2, 3, 4, 5]);
    const output = reverseBetween(input, 2, 4);
    const expected = convertArrayToList([1, 4, 3, 2, 5]);
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const input = convertArrayToList([5]);
    const output = reverseBetween(input, 1, 1);
    const expected = convertArrayToList([5]);
    expect(output).toEqual(expected);
  });
});

```
