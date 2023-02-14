---
title: Group Anagrams
author: chaneesong
date: 2023-02-14 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

## 문제 설명

문자열로 이루어진 배열이 주어질 때, 애너그램 단위로 그룹화 묶어라.
정달을 반환할 때에는 정렬은 고려하지 않아도 된다.

애너그램은 일반적으로 모든 오리지널 문자를 한 번씩만 사용하여 다른 단어나 구를 재배치하여 형성된 단어나 구이다.

예를 들면, eat과 tea는 같은 애너그램 그룹이다.

## 해결 방법

1. 애너그램을 그룹화 하기 위해 현재 단어를 오름차순으로 정렬한 단어를 key 값으로 사용한다.
    - example: eat => aet
2. 각 애너그램 key의 value는 배열로 세팅한다.
3. 각 단어들을 key값에 따라 value에 넣는다.
4. object 자료구조에서 value를 추출해 리턴한다.

## 풀이 코드

```typescript
function groupAnagrams(strs: string[]): string[][] {
  const anagrams = {};

  for (let str of strs) {
    const key = str.split('').sort().join('');

    if (!anagrams[key]) anagrams[key] = [];
    anagrams[key].push(str);
  }

  return Object.values(anagrams);
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { groupAnagrams } from '.';

describe(' description', () => {
  test('example test case', () => {
    const input = ['eat', 'tea', 'tan', 'ate', 'nat', 'bat'];
    const output = groupAnagrams(input);
    const expectedOutput = [
      expect.arrayContaining(['eat', 'tea', 'ate']),
      expect.arrayContaining(['tan', 'nat']),
      expect.arrayContaining(['bat']),
    ];
    expect(output).toEqual(expect.arrayContaining(expectedOutput));
  });

  test('none input test', () => {
    const result = groupAnagrams(['']);
    expect(result).toEqual([['']]);
  });

  test('one input test', () => {
    const result = groupAnagrams(['a']);
    expect(result).toEqual([['a']]);
  });
});
```

## 구현 후

이 문제는 사실 별로 어렵지는 않았다.  
그런데 어떠한 정렬 상태도 테스트케이스를 통과해야되기 때문에 테스트 케이스를 작성하는데 어려움이 있었다.  
테스트케이스 작성은 처음이다보니 어떠한 매처들이 있는지 찾기 힘들었고, chatGpt를 활용했다.  
chatGpt를 활용하니 arrayContaining이라는 매처를 찾아줬고, 아주 쉽게 테스트케이스를 작성할 수 있었다.  
구글 검색으로는 어떻게 검색해야 될지 몰라서 헤맸는데, 아무리 봐도 Gpt는 놀라움의 연속이다.  
사실 처음부터 맞는 테스트케이스를 찾아준 것은 아니지만 여러 번 수정을 해주니 원하는 정답을 찾아줬다.  
chatGpt와 소통 과정을 한 번 포스팅 해야겠다.  
