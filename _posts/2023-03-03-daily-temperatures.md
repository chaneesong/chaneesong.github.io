---
title: Daily Temperatures
author: chaneesong
date: 2023-03-03 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/description/)

## 문제 설명

일일 온도를 나타내는 정수 배열이 주어질 때, 해당 날짜로부터 몇일 뒤에 따뜻해지는지를 나타내는 `array[i]`를 가진 배열 `array`를 반환하라. 미래에 따뜻해지는 날이 없다면, `answer[i] == 0`으로 처리하라.

## 해결 방법

스택을 활용하여 해결한다.

1. `answer`배열을  주어진 배열의 길이만큼 0으로 초기화한다.
2. 배열을 순회하면서 해당 인덱스를 스택에 넣기 전 현재 온도와 스택의 top 인덱스의 온도를 비교한다.
3. 현재 온도가 더 크다면, answer의 인덱스를 `현재 index - stack top index`로 변경한다.
4. 현재 인덱스를 스택에 `push`한다.

## 풀이 코드

```typescript
function dailyTemperatures(temperatures: number[]): number[] {
  const stack: number[] = [];
  const answer = new Array<number>(temperatures.length).fill(0);

  for (let i = 0; i < temperatures.length; i++) {
    while (stack && temperatures[i] > temperatures[stack.at(-1)]) {
      const top = stack.pop();
      answer[top] = i - top;
    }
    stack.push(i);
  }
  return answer;
}
```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const input = [73, 74, 75, 71, 69, 72, 76, 73];
    const output = dailyTemperatures(input);
    const expected = [1, 1, 4, 2, 1, 1, 0, 0];
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const input = [30, 40, 50, 60];
    const output = dailyTemperatures(input);
    const expected = [1, 1, 1, 0];
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const input = [30, 60, 90];
    const output = dailyTemperatures(input);
    const expected = [1, 1, 0];
    expect(output).toEqual(expected);
  });
});
```
