---
title: Combination Sum
author: chaneesong
date: 2023-03-17 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [38. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

## 문제 설명

중복되지 않는 정수 배열 `canditates`와 타겟 정수 target이 주어질 때, 선택한 숫자의 합이 `target`이 되는 `candidates`의 고유 조합 리스트를 반환하세요. 정렬은 신경쓰지 않습니다.

## 해결 방법

해당 문제는 [Combinations](https://chaneesong.github.io/posts/combinations/)를 응용한 문제로 링크의 문제를 제대로 이해했다면 풀이하는데 어려움이 없다.  
`Combinations`와 다른 점은 같은 숫자가 중복 될 수 있는 조합이라는 점과 타겟이 정해져있다는 것이다. 그렇기 때문에 해당 문제에서는 이전과 다르게 재귀 호출을 진행 할 때 `depth`를 올리지 않고, `target`에서 현재 값을 빼는 형식으로 진행하면 풀이가 끝난다.

동작 방식은 이전 Comninations와 거의 동일하기 때문에 생략한다.

## 풀이 코드

```typescript
function combinationSum(
  candidates: number[],
  target: number
): number[][] {
  const answer: number[][] = [];
  const dfs = (elements: number[], sum: number, depth: number) => {
    if (sum < 0) {
      return;
    }
    if (sum === 0) {
      answer.push([...elements]);
      return;
    }

    for (let i = depth; i < candidates.length; i++) {
      elements.push(candidates[i]);
      dfs(elements, sum - candidates[i], i);
      elements.pop();
    }
  };

  dfs([], target, 0);

  return answer;
}
```

## 테스트 코드

```typescript
describe('39. Combination Sum', () => {
  test('example test 1', () => {
    const output = combinationSum([2, 3, 6, 7], 7);
    const expected = [[7], [2, 2, 3]];
    expect(output).toEqual(expect.arrayContaining(expected));
  });
  test('example test 1', () => {
    const output = combinationSum([2, 3, 5], 8);
    const expected = [
      [2, 2, 2, 2],
      [2, 3, 3],
      [3, 5],
    ];
    expect(output).toEqual(expect.arrayContaining(expected));
  });
  test('example test 1', () => {
    const output = combinationSum([2], 1);
    const expected = [];
    expect(output).toEqual(expected);
  });
});
```
