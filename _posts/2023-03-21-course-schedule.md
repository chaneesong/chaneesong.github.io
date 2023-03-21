---
title: Course Schedule
author: chaneesong
date: 2023-03-21 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [207. Course Schedule](https://leetcode.com/problems/course-schedule/description/)

## 문제 설명

수강해야하는 `numCourses` 코스의 총 개수는 `0`부터 `numCourses - 1`까지 표시됩니다. 배열 `prerequisites`가 주어지며, 여기서 `prerequisites[i] = [a, b]`는 `a`를 수강하려면 `b`를 먼저 수강해야함을 나타냅니다.

- 예를 들어, [0, 1]쌍은 코스 0을 수강하려면 먼저 코스 1을 수강해야함을 나타냅니다.

모든 코스를 끝낼 수 있으면 `true`를 반환하세요. 그렇지 않으면 `false`를 반환하세요.

## 해결 방법

그래프로 코스를 연결하여 순환 구조(Cycle)를 가지는지 확인하여 문제를 해결할 수 있다. 만약 Cycle 가지면 해당 코스는 영원히 끝낼 수 없음으로 `false`를 리턴하면 된다.

1. `graph = Map<number, number[]>`을 사용하여 그래프를 구성한다.
2. Cycle을 탐색하기 위한 `traced = Set<number>`과 이미 방문한 적이 있다면 넘어가기 위한 `visited = Set<number>` = 을 추가로 구성한다.
3. 이제 그래프의 모든 `key`를 대상으로 탐색을 진행하는데, 진행 방법은 아래와 같다.
   1. `traced`에 현재 방문한 노드가 존재하면 사이클로 간주하고 `false`를 리턴한다.
   2. `visited`에 방문한 노드가 존재하면 `true`를 리턴하고 넘어간다.
   3. 위 두 과정에 해당하지 않으면 먼저 방문 기록을 `traced`에 남긴다.
   4. `key`에 해당하는 `value`를 `graph`에서 얻어와 리스트를 순회하며 재귀 탐색을 진행한다.
   5. 재귀 탐색이 정상적으로 끝난 경우 다음 탐색을 위해 `traced`에서 방문 기록을 삭제하고, `visited`에 기록을 남긴다.
4. 위 과정을 끝까지 반복하면 Cycle을 판별할 수 있다.

## 풀이 코드

```typescript
function canFinish(numCourses: number, prerequisites: number[][]): boolean {
  const graph = new Map<number, number[]>();
  const traced = new Set<number>();
  const visited = new Set<number>();

  for (let [src, dst] of prerequisites) {
    if (!graph.has(src)) graph.set(src, []);
    graph.get(src).push(dst);
  }

  const dfs = (src: number): boolean => {
    if (traced.has(src)) return false;
    if (visited.has(src)) return true;

    traced.add(src);
    for (let dst of graph.get(src) || []) {
      if (!dfs(dst)) return false;
    }
    traced.delete(src);
    visited.add(src);
    return true;
  };

  for (let key of graph.keys()) {
    if (!dfs(key)) return false;
  }

  return true;
}
```

## 테스트 코드

```typescript
describe('207. Course Schedule', () => {
  test('example test 1', () => {
    const output = canFinish(2, [[1, 0]]);
    const expected = true;
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const output = canFinish(2, [
      [1, 0],
      [0, 1],
    ]);
    const expected = false;
    expect(output).toBe(expected);
  });
});
```
