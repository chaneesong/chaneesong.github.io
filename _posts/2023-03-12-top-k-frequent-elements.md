---
title: Top K Frequent Elements
author: chaneesong
date: 2023-03-12 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

## 문제 설명

정수 배열 `nums`와 정수 `k`가 주어질 때, `k`개의 가장 자주 등장하는 요소를 반환합니다. 정답은 정렬이 필요 없습니다.

## 해결 방법

배열의 원소 `num`과 등장 횟수 `frequent`가 있으면 `<num: frequent>`로 맵을 구성한 후 해당 엔트리를 `frequent` 기준으로 내림차순 한 후 `k`만큼 잘라서 반환한다.

## 풀이 코드

```typescript
function topKFrequent(nums: number[], k: number): number[] {
  const freq = new Map<number, number>();

  for (let i = 0; i < nums.length; i++) {
    freq.set(nums[i], freq.get(nums[i]) + 1 || 1);
  }

  const answer = Array.from(freq.entries()).sort((a, b) => b[1] - a[1]);

  return answer.slice(0, k).map((element) => Number(element[0]));
}

```

## 테스트 코드

```typescript
describe('347. Top K Frequent Elements', () => {
  test('example test 1', () => {
    const output = topKFrequent([1, 1, 1, 2, 2, 3], 2);
    const expected = [1, 2];
    expect(output).toEqual(expected);
  });
  test('example test 2', () => {
    const output = topKFrequent([1], 1);
    const expected = [1];
    expect(output).toEqual(expected);
  });
});
```
