---
title: Swap Nodes in Pairs
author: chaneesong
date: 2023-02-26 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

## 문제 설명

연결 리스트가 주어질 때, 모든 두 개의 인접 노드를 스왑하고, 스왑한 노드들의 `head`를 리턴하라.

이 문제는 리스트 노드의 `values`를 바꾸지 않고 해결해야 한다(`node` 자체를 바꿔야 한다.).

## 해결 방법

재귀를 이용해 해결한다.

1. 재귀의 종료 조건은 연결리스트의 끝까지 도달 했을 때이다.
2. `head.next`를 변수 `p`에저장한다.
3. `head.next`의 값을 `recursive return` 값으로 변경한다.
4. `p.next`를 `head`로 변경한다.

![movement](/assets/img/algorithm/linked-list/swap-nodes-in-pairs/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function swapPairs(head: ListNode | null): ListNode | null {
  if (!head || !head.next) return head;

  const p = head.next;

  head.next = swapPairs(p.next);
  p.next = head;
  return p;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const input = convertArrayToList([1, 2, 3, 4]);
    const output = swapPairs(input);
    const expected = convertArrayToList([2, 1, 4, 3]);
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const input = null;
    const output = swapPairs(input);
    const expected = null;
    expect(output).toEqual(expected);
  });

  test('example test 3', () => {
    const input = convertArrayToList([1]);
    const output = swapPairs(input);
    const expected = convertArrayToList([1]);
    expect(output).toEqual(expected);
  });
});
```

## 구현 후

실제 동작하는 과정과 사람이 보기 편한 이미지는 좀 다르다.

![human](/assets/img/algorithm/linked-list/swap-nodes-in-pairs/human-image.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

![real](/assets/img/algorithm/linked-list/swap-nodes-in-pairs/real-image.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

그렇기 때문에 연결 리스트에 재귀를 더하면 이해하기 많이 힘들다.
