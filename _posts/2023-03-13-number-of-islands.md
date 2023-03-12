---
title: Number of Islands
author: chaneesong
date: 2023-03-13 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

## 문제 설명

`'1'`(땅)과 `'0'`(물)로 구성된 지도를 나타내는 `m x n` 2차원 배열이 주어질 때, 섬의 개수를 리턴하세요.

섬은 물로 둘러싸여 있으며 땅이 수직 또는 수평으로 연결되어 구성된다. 배열의 네 방향의 가장자리는 물로 둘러싸여 있다고 가정합니다.

## 해결 방법

재귀를 활용한 dfs 탐색을 진행한다.

1. 2차원 배열을 순차적으로 돌면서 `1`을 만났을 때, 재귀 탐색을 시작한다.
2. 재귀 탐색은 다음과 같은 조건이 만족하면 `1`을 `0`으로 변경한다.
   - `0 <= row < grid.length`
   - `0 <= column < grid[0].length`
3. 그 외의 경우 즉, 배열의 범위를 벗어난 경우 또는 물에 위치한 경우에는 재귀 탐색을 종료한다.

### 동작 방식

![movement](/assets/img/algorithm/number-of-islands-movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function numIslands(grid: string[][]): number {
  let answer = 0;

  const dfs = (row: number, col: number): void => {
    if (
      row < 0 ||
      row >= grid.length ||
      col < 0 ||
      col >= grid[0].length ||
      grid[row][col] !== '1'
    ) {
      return;
    }

    grid[row][col] = '0';

    dfs(row + 1, col);
    dfs(row - 1, col);
    dfs(row, col + 1);
    dfs(row, col - 1);
  };

  for (let i = 0; i < grid.length; i++) {
    for (let j = 0; j < grid[0].length; j++) {
      if (grid[i][j] === '1') {
        dfs(i, j);
        answer++;
      }
    }
  }
  return answer;
}
```

## 테스트 코드

```typescript
describe('200. Number of Islands', () => {
  test('example test 1', () => {
    const grid = [
      ['1', '1', '1', '1', '0'],
      ['1', '1', '0', '1', '0'],
      ['1', '1', '0', '0', '0'],
      ['0', '0', '0', '0', '0'],
    ];
    const output = numIslands(grid);
    const expected = 1;
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const grid = [
      ['1', '1', '0', '0', '0'],
      ['1', '1', '0', '0', '0'],
      ['0', '0', '1', '0', '0'],
      ['0', '0', '0', '1', '1'],
    ];
    const output = numIslands(grid);
    const expected = 3;
    expect(output).toBe(expected);
  });
});
```
