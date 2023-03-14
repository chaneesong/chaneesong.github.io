---
title: Letter Combinations of a Phone Number
author: chaneesong
date: 2023-03-14 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

## 문제 설명

2-9까지 숫자에 포함 된 문자열이 주어질 때, 주어진 숫자로 표현가능한 모든 문자 조합을 반환하세요. 어떤 순서로든 답을 반환하세요.

![phone](/assets/img/algorithm/letter-combinations-of-a-phone-number/phone.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 해결 방법

dfs 탐색을 통해 가능한 모든 조합을 탐색한다.

1. 숫자에 포함된 문자열은 `<숫자: 문자열>`로 매핑한다.
2. dfs는 주어진 `digits`의 길이와 현재 조합된 문자열의 길이가 같을 때 종료한다.
3. 해당 숫자의 문자열을 모두 순회하며 재귀를 진행한다.

### 탐색이 진행되는 과정

![movement](/assets/img/algorithm/letter-combinations-of-a-phone-number/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
export function letterCombinations(digits: string): string[] {
  const dfs = (index: number, path: string): void => {
    if (digits.length === path.length) {
      answer.push(path);
      return;
    }

    for (let letter of phone[digits[index]]) {
      dfs(index + 1, path + letter);
    }
  };

  const answer: string[] = [];
  const phone = {
    2: 'abc',
    3: 'def',
    4: 'ghi',
    5: 'jkl',
    6: 'mno',
    7: 'pqrs',
    8: 'tuv',
    9: 'wxyz',
  };

  if (!digits) return [];
  dfs(0, '');
  return answer;
}
```

## 테스트 코드

```typescript
import { letterCombinations } from '.';

describe('17. Letter Combinations of a Phone Number', () => {
  test('example test 1', () => {
    const output = letterCombinations('23');
    const expected = expect.arrayContaining([
      'ad',
      'ae',
      'af',
      'bd',
      'be',
      'bf',
      'cd',
      'ce',
      'cf',
    ]);
    expect(output).toEqual(expected);
  });
  test('example test 2', () => {
    const output = letterCombinations('');
    const expected = [];
    expect(output).toEqual(expected);
  });
  test('example test 3', () => {
    const output = letterCombinations('2');
    const expected = expect.arrayContaining(['a', 'b', 'c']);
    expect(output).toEqual(expected);
  });
});
```
