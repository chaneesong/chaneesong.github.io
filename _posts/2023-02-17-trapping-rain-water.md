---
title: Trapping Rain Water
author: chaneesong
date: 2023-02-17 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/description/)

## 문제 설명

각각의 막대의 너비가 1인 높이를 나타내는 자연수 n이 주어질 때, 비가 온 후 물을 얼마나 담을 수 있는지 계산하라.

## 해결 방법

**투 포인터**를 활용하여 왼쪽과 오른쪽에서 현재 높이와 최대 높이의 차이 만큼을 더해나간다.  

1. 양 쪽 모두 최대 높이와 현재 높이를 비교해 최대 높이를 갱신한다.
2. 최대 높이를 서로 비교해 너 높은 쪽으로 낮은 쪽이 이동하는 방식으로 진행한다.
3. 진행하면서 현재 높이가 최대 높이보다 낮으면 빗물을 받을 수 있으므로 결과값에 더한다.

![movement](/assets/img/algorithm/trapping-rain-water/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
export function trap(height: number[]): number {
  let answer = 0;
  let left = 0;
  let right = height.length - 1;
  let leftMax = height[left];
  let rightMax = height[right];

  while (left < right) {
    leftMax = Math.max(leftMax, height[left]);
    rightMax = Math.max(rightMax, height[right]);

    if (leftMax <= rightMax) {
      answer += leftMax - height[left];
      left++;
    } else {
      answer += rightMax - height[right];
      right--;
    }
  }

  return answer;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { trap } from '.';

describe(' description', () => {
  test(' test example 1', () => {
    const output = trap([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]);
    const expected = 6;
    expect(output).toEqual(6);
  });

  test(' test example 2', () => {
    const output = trap([4, 2, 0, 3, 2, 5]);
    const expected = 9;
    expect(output).toEqual(9);
  });
});
```
