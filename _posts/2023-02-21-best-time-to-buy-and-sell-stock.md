---
title: Best Time to Buy and Sell Stock
author: chaneesong
date: 2023-02-21 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

## 문제 설명

`i`번 째 날의 주식 가격이 `prices[i]`인 `prices` 배열이 주어진다.

특정 날짜 하루를 지정 해서 주식을 사고, 주식을 산 날보다 미래의 다른 날짜에 주식을 팔아서 최대의 이익을 내고 싶다.

해당 거래에서 달성 할 수 있는 가장 큰 이익을 리턴하고, 이익을 얻을 수 없으면 0을 리턴하라.

## 해결 방법

최솟값을 갱신해 나가면서 현재 날짜와 최솟값의 차이를 계산한다.

## 풀이 코드

```typescript
function maxProfit(prices: number[]): number {
  let answer = 0;
  let minPrice = Infinity;

  for (let i = 0; i < prices.length; i++) {
    minPrice = Math.min(minPrice, prices[i]);

    answer = Math.max(answer, prices[i] - minPrice);
  }

  return answer;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { maxProfit } from '.';

describe(' description', () => {
  test('example test 1', () => {
    const output = maxProfit([7, 1, 5, 3, 6, 4]);
    const expected = 5;
    expect(output).toEqual(expected);
  });

  test('example test', () => {
    const output = maxProfit([7, 6, 4, 3, 1]);
    const expected = 0;
    expect(output).toEqual(expected);
  });
});
```

## 구현 후

최솟값을 저장하기 위한 변수를 초기화 할 때, 보통 해당 언어에서 가장 큰 값을 저장하기 마련이다. 그런데 자바스크립트에서 `Infininy`가 가장 큰 수 인 줄 알았지만, 다른 답을 보면서 최댓값을 구할 때 여러가지 방법이 있다는 것을 알았다.  

1. `Number.MAX_VALUE`는 부동소수점을 사용하여 나타낼 수 있는 가장 큰 수
2. `Number.MAX_SAFE_INTEGER`는 부동소수점을 사용하여 정확하게 나타낼 수 있는 수 중에서 가장 큰 수
3. `Ifinity`는 말 그대로 무한대이며, 이 보다 큰 숫자는 존재하지 않는다(표현이 불가능한 수를 나타냄).
