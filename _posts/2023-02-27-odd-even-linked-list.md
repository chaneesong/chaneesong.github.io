---
title: Odd Even Linked List
author: chaneesong
date: 2023-02-27 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/description/)

## 문제 설명

단일 연결리스트의 `head`가 주어질 때, 홀수 번째 노드 뒤에 짝수 번째 노드가 오도록 그룹화 한 후, 재정렬 된 리스트를 반환하라.

첫 번째 노드는 홀수로 간주되고, 두 번째 노드는 짝수 번째 노드로 간주한다.  

**주의사항** - 짝수와 홀수를 구분하여 나눌 때 그룹 내에서 상대적 순서는 입력 순서 그대로 유지한다.

공간 복잡도는 `O(1)`, 시간 복잡도 `O(n)`의 복잡도로 해결해야 한다.

## 해결 방법

홀수 노드와 짝수노드를 각각 연결 한 후, 홀수 노드의 마지막에 짝수 노드를 붙힌다.

![movement](/assets/img/algorithm/odd-even-linked-list/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function oddEvenList(head: ListNode | null): ListNode | null {
  if (!head) return head;

  const evenHead = head.next;
  let odd = head;
  let even = evenHead;

  while (even && even.next) {
    odd.next = odd.next.next;
    even.next = even.next.next;
    odd = odd.next;
    even = even.next;
  }

  odd.next = evenHead;

  return head;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const input = convertArrayToList([1, 2, 3, 4, 5]);
    const output = oddEvenList(input);
    const expected = convertArrayToList([1, 3, 5, 2, 4]);
    expect(output).toEqual(expected);
  });

  test('example test 1', () => {
    const input = convertArrayToList([2, 1, 3, 5, 6, 4, 7]);
    const output = oddEvenList(input);
    const expected = convertArrayToList([2, 3, 6, 7, 1, 5, 4]);
    expect(output).toEqual(expected);
  });
});
```
