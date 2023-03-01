---
title: Valid Parentheses
author: chaneesong
date: 2023-03-01 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

## 문제 설명

`'(', ')', '{', '}', '[', ']'`로 이루어진 문자열이 주어질 때, 입력 문자열이 유효한지 판단하라.

**입력 문자열이 유효한 경우**:

열린 괄호는 같은 타입의 닫히는 괄호로 닫혀야 한다.  
열린 괄호는 정확한 위치에서 닫혀야한다.  
모든 닫힌 괄호는 같은 타입의 열린 괄호가 존재한다.

## 해결 방법

닫히는 괄호: 열리는 괄호를 `key: value`로 가지는 hashmap을 구성하고, 열리는 괄호는 스택에 넣고 닫히는 괄호는 스택의 `top`과 비교하여 유효한지 판별한다.

## 풀이 코드

```typescript
function isValid(s: string): boolean {
  const stack = [];
  const table = {
    ']': '[',
    '}': '{',
    ')': '(',
  };

  for (let char of s) {
    if (!table[char]) {
      stack.push(char);
    } else if (!stack || table[char] !== stack.pop()) {
      return false;
    }
  }

  return stack.length === 0;
}
```

## 테스트 코드

```typescript
import { isValid } from '.';

describe(' description', () => {
  test('example test 1', () => {
    const output = isValid('()[]{}');
    expect(output).toBe(true);
  });

  test('example test 1', () => {
    const output = isValid('()');
    expect(output).toBe(true);
  });

  test('example test 1', () => {
    const output = isValid('(]');
    expect(output).toBe(false);
  });
});
```
