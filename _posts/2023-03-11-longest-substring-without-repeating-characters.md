---
title: Longest Substring Without Repeating Characters
author: chaneesong
date: 2023-03-11 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

## 문제 설명

문자열 `s`가 주어질 때 중복 문자가 없는 가장 긴 부분 문자열을 찾으세요.

## 해결 방법

투 포인터와 중복 문자를 판별하는 `HashMap`을 이용한다.

1. 문자열 왼쪽에서 시작하는 `left`와 `right` 포인터를 만든다.
2. 오른쪽 포인터는 계속해서 한 칸씩 이동하면서 `HashMap`에 현재 위치한 문자의 개수를 기록한다.
3. 왼쪽 포인터는 현재 `left`와 `right`사이에서 `right`에 위치한 문자가 2개 이상이면(중복 문자가 발생한 경우), `left`는 중복 문자 이후로 이동하면서 `HashMap`에서 `left` 위치에 해당하는 문자 개수를 하나 씩 감소시킨다.
4. 이동한 `left`와 `right`의 길이와 `answer`를 비교해 더 큰값을 `answer`에 넣는다.

![movement](/assets/img/algorithm/hashmap/longest-substring-without-repeating-characters/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function lengthOfLongestSubstring(s: string): number {
  const used = new Map<string, number>();
  let left = 0;
  let answer = 0;

  for (let right = 0; right < s.length; right++) {
    used.set(s[right], used.get(s[right]) + 1 || 1);

    while (used.get(s[right]) > 1) {
      const start = s[left];
      used.set(start, used.get(start) - 1);
      left++;
    }

    answer = Math.max(answer, right - left + 1);
  }

  return answer;
}
```

## 테스트 코드

```typescript
describe('3. Longest Substring Without Repeating Characters', () => {
  test('example test 1', () => {
    const input = 'abcabcbb';
    const output = lengthOfLongestSubstring(input);
    const expected = 3;
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const input = 'bbbbb';
    const output = lengthOfLongestSubstring(input);
    const expected = 1;
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const input = 'pwwkew';
    const output = lengthOfLongestSubstring(input);
    const expected = 3;
    expect(output).toBe(expected);
  });
});
```
