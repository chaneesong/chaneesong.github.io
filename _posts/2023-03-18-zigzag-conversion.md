---
title: Zigzag Conversion
author: chaneesong
date: 2023-03-18 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [6. Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/description/)

## 문제 설명

문자열 `"PAYPALISHIRING"`을 주어진 행 수에 대해 다음과 같은 지그재그 패턴으로 작성합니다.

```text
P   A   H   N
A P L S I I G
Y   I   R
```

그런 다음 한줄 씩 읽습니다: `"PAHNAPLSIIGYIR"`  
문자열과 변환을 위한 행의 수가 주어지면 변환을 수항해는 코드를 작성하세요.

```text
string convert(string s, int numRows);

```

## 해결 방법

해당 문제는 규칙을 찾는 것이 중요하다. 규칙은 아래와 같다.

1. 시작 지점(`row = 0`)에서 다음 `row`가 `0`인 문자가 나오기 전까지 개수를 파악한다(**Cycle**이라 칭함).
2. 최대 행을 변곡점이라 칭하고(`numRows - 1`), 1씩 증가하다가 변곡점부터는 다시 1씩 감소해야한다.
3. 위의 규칙을 바탕으로 `0 ~ numRows`까지 1씩 증가하고, `numRows + 1 ~ Cycle`은 1씩 감소한다.

이제 위의 규칙을 바탕으로 정답 문자열을 만들어야한다. 만들 때는 `Map<row, string>`을 활용하여 만든다.  

1. 먼저 `Cycle`은 `2 * (numRows - 1)`을 통해 구할 수 있다.
2. 위의 규칙에서는 변곡점 전까지 +1하고 변곡점 이후로 -1을 한다고 했지만, 이것을 코드로 작성하기 위해서는 약간의 트릭이 필요했다.
3. 일단, 문자열을 반복하면서 현재 인덱스를 Cycle로 나눈 나머지를 구한다.
4. 해당 나머지가 `numRows`보다 작다면 그대로 `key`로 사용하고, 크다면 `((i % rule) * (numRows - 1) - 2) % numRows`과 같이 수식을 이용해서 `-1`씩 감소시킨다.
5. 이것을 반복하면 `Map`에 해당 `row`에 해당하는 문자열이 만들어지고, 해당 문자열들을 순서대로 합치면 정답을 만들 수 있다.

## 풀이 코드

```typescript
function convert(s: string, numRows: number): string {
  if (numRows === 1) return s;
  let answer = '';
  const conversion = new Map<number, string>();
  const rule = 2 * (numRows - 1);

  for (let i = 0; i < s.length; i++) {
    const key =
      i % rule <= numRows - 1
        ? i % rule
        : ((i % rule) * (numRows - 1) - 2) % numRows;
    const exString = conversion.get(key) ?? '';
    conversion.set(key, exString + s[i]);
  }

  for (let i = 0; i < conversion.size; i++) {
    answer = answer + conversion.get(i);
  }
  return answer;
}
```

## 테스트 코드

```typescript
describe('6. Zigzag Conversion', () => {
  test('example test 1', () => {
    const output = convert('PAYPALISHIRING', 3);
    const expected = 'PAHNAPLSIIGYIR';
    expect(output).toEqual(expected);
  });
  test('example test 2', () => {
    const output = convert('PAYPALISHIRING', 4);
    const expected = 'PINALSIGYAHRPI';
    expect(output).toEqual(expected);
  });
  test('example test 3', () => {
    const output = convert('A', 1);
    const expected = 'A';
    expect(output).toEqual(expected);
  });
});
```

## 구현 후

수식을 사용해서 해보고 싶어서 사용했지만 애초에 numRows를 기준으로 증가, 감소를 했어도 상관없었을 것 같긴 하다. 또한, 이런 구현 문제 같은 경우에는 문제에서 요구하는 원리만 파악하면 쉽게 풀 수 있다. 하지만 구현까지 가기 위한 접근 방식이 더 중요하다는 생각이 들었다.
