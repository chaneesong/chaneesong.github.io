---
title: Jewels and Stones
author: chaneesong
date: 2023-03-10 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [771. Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)

## 문제 설명

보석인 돌의 종류를 나타내는 `jewels` 문자열이 주어지고, 가지고 있는 돌의 종류를 나타내는 `stones`가 주어진다. 스톤의 각 문자는 내가 가진 스톤의 종류를 나타낸다. 보유하고 있는 스톤 중 보석이 몇 개인지 알고싶다.

문자는 대소문자를 구분하므로 `"a"`와 `"A"`는 다른 문자를 나타낸다.

## 해결 방법

`Map`자료구조를 사용하여 해결한다.

1. `stones`를 순회하며 Map에 `(stone: value + 1)`을 저장한다.
   - `Map`에 stone이 없으면 0으로 초기화한다.
2. `jewels`를 순회하며 `Map`에 요소가 있으면 결과 값에 더한다.

## 풀이 코드

```typescript
function numJewelsInStones(jewels: string, stones: string): number {
  let answer = 0;
  const frequency = new Map<string, number>();

  for (let stone of stones) {
    if (!frequency.has(stone)) frequency.set(stone, 0);
    frequency.set(stone, frequency.get(stone) + 1);
  }

  for (let jewel of jewels) {
    if (frequency.has(jewel)) answer += frequency.get(jewel);
  }
  return answer;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const jewels = 'aA';
    const stones = 'aAAbbbb';
    const output = numJewelsInStones(jewels, stones);
    const expected = 3;
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const jewels = 'z';
    const stones = 'ZZZ';
    const output = numJewelsInStones(jewels, stones);
    const expected = 0;
    expect(output).toBe(expected);
  });
});
```
