---
title: Array Partition
author: chaneesong
date: 2023-02-19 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [561. Array Partition](https://leetcode.com/problems/array-partition/description/)

## 문제 설명

짝수개의 정수를 가진 배열이 주어질 때, 이 정수들을 n개의 페어 `(a1, b1), (a2, b2), ..., (aN, bN)`로 만들어 `min(a, b)`의 최대 합을 리턴하라.

## 해결 방법

정렬된 상태에서 인접한 요소와 페어를 만들게 되면 페어의 최솟값들의 합이 최대합이 된다.  
또, 정렬을 한 상태에서 페어를 만들게 되면 최솟값이 항상 왼쪽에 위치하기 때문에 인덱스 0부터 2칸 씩 이동하면 최솟값이 된다.

테스트 케이스를 예로 들면,  

1. `[6, 2, 6, 5, 1, 2]`를 정렬한다.
2. `[1, 2, 2, 5, 6, 6]`의 2n 번 째 값을 모두 더한다.
3. `1 + 2 + 6`이므로 답은 9가 된다.

## 풀이 코드

```typescript
function arrayPairSum(nums: number[]): number {
  let answer = 0;
  const sortedNums = nums.sort((a, b) => a - b);

  for (let i = 0; i < sortedNums.length; i += 2) {
    answer += sortedNums[i];
  }

  return answer;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { arrayPairSum } from '.';

describe(' description', () => {
  test('example test 1', () => {
    const output = arrayPairSum([1, 4, 3, 2]);
    const expected = 4;
    expect(output).toEqual(expected);
  });

  test('example test', () => {
    const output = arrayPairSum([6, 2, 6, 5, 1, 2]);
    const expected = 9;
    expect(output).toEqual(expected);
  });
});
```
