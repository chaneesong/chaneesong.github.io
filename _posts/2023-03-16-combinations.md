---
title: Combinations
author: chaneesong
date: 2023-03-16 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [77. Combinations](https://leetcode.com/problems/combinations/)

## 문제 설명

두 정수 `n`과 `k`가 주어질 때, `[1, n]`의 범위에서 `k`개의 가능한 모든 조합을 리턴하세요.  
정답은 어떠한 어떠한 순서로도 반환할 수 있습니다.

## 해결 방법

`dfs`(재귀)를 활용하여 모든 조합을 구한다.

1. `dfs`는 현재 조합을 저장하는 배열(`elements`)과 시작위치를 나타내는 `depth`를 매개변수로 받는다.
2. `dfs`는 `elements`의 길이가 `k`가 되었을 때 종료한다.
3. 시작 위치부터 순회하며 `elements`에 `index`를 넣는다.
4. 현재 조합의 다음 값을 찾기 위해 재귀에 들어간다.
   - 재귀는 현재 `elements`와 `i + 1`을 인자로 받는다.
5. 백트래킹이 끝난경우 `elements`에서 값을 `pop`한다.

배열이 아닌 반복으로 진행되지만 이해를 돕기 위해 아래의 동작방식은 배열 형태를 나타내도록 하겠습니다.

![movement](/assets/img/algorithm/combinations-movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

실제로는 뒤에 자잘한 동작을 조금 더 하지만 의미없는 움직임이기 때문에 제외했다.

## 풀이 코드

```typescript
function combine(n: number, k: number): number[][] {
  const answer: number[][] = [];
  const dfs = (elements: number[], depth: number) => {
    if (elements.length === k) {
      answer.push(elements.slice());
      return;
    }

    for (let i = depth; i < n + 1; i++) {
      elements.push(i);
      dfs(elements, i + 1);
      elements.pop();
    }
  };

  dfs([], 1);
  return answer;
}
```

## 테스트 코드

```typescript
describe('77. Combinations', () => {
  test('example test 1', () => {
    const output = combine(4, 2);
    const expected = [
      [1, 2],
      [1, 3],
      [1, 4],
      [2, 3],
      [2, 4],
      [3, 4],
    ];
    expect(output).toEqual(expect.arrayContaining(expected));
  });
  test('example test 2', () => {
    const output = combine(1, 1);
    const expected = [[1]];
    expect(output).toEqual(expect.arrayContaining(expected));
  });
});
```
