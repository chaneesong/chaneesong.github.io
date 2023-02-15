---
title: Longest Palindromic Substring
author: chaneesong
date: 2023-02-15 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)

## 문제 설명

문자열 s가 주어질 때, 부분 문자열 중 가장 긴 팰린드롬을 리턴하라.

## 해결 방법

1. 부분 문자열의 팰린드롬을 확인하는 `expand` 함수를 정의한다.
    - `expand`는 left와 right를 투포인터로 받아 양쪽으로 확장하면서 팰린드롬을 체크한다.
2. 입력받은 문자열이 자체 팰린드롬인지 검사한다.
    - s의 길이가 2 미만 또는 전체가 팰린드롬 일 경우 문자열 전체를 반환한다.
3. 문자열을 순회하면서 최장 길이 부분 문자열 팰린드롬을 탐색한다.
    - 현재 최장길이와 현재 index부터 팰린드롬을 탐색하여 비교한다.
    - 부분 문자열을 탐색할 때 홀수의 길이와 짝수의 길이는 시작 위치가 다르므로 각각 탐색한다.

index가 2일 때 탐색 방법이다.  

### 홀수 탐색

![odd](/assets/img/algorithm/longest-palindromic-substring/odd.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

### 짝수 탐색(방식을 보여주는 것일 뿐 실제로 다르다면 다음으로 넘어가지 않는다.)

![even](/assets/img/algorithm/longest-palindromic-substring/even.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function isPalindrome(s: string): boolean {
  for (let i = 0; i < Math.floor(s.length / 2); i++) {
    if (s[i] !== s[s.length - 1 - i]) return false;
  }
  return true;
}

export function longestPalindrome(s: string): string {
  const expand = (left: number, right: number): string => {
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left -= 1;
      right += 1;
    }
    return s.slice(left + 1, right);
  };

  let answer = '';

  if (s.length < 2 || isPalindrome(s)) return s;

  for (let i = 0; i < s.length - 1; i++) {
    answer = [answer, expand(i, i + 1), expand(i, i + 2)].sort(
      (a, b) => b.length - a.length
    )[0];
  }

  return answer;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { longestPalindrome } from '.';

describe(' description', () => {
  test('example test case1', () => {
    const output = longestPalindrome('babad');
    const expectedOutput = 'bab';
    expect(output).toEqual(expectedOutput);
  });

  test('example test case2', () => {
    const output = longestPalindrome('cbbd');
    const expectedOutput = 'bb';
    expect(output).toEqual(expectedOutput);
  });
});
```
