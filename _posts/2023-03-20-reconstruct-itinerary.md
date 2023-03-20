---
title: Reconstruct Itinerary
author: chaneesong
date: 2023-03-20 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/description/)

## 문제 설명

`tickets[i] = [from, to]`로 출발 및 도착 공항을 나타내는 항공권 `tickets`가 주어집니다. 여정을 순서대로 정렬하고 반환하세요.

모든 항공권은 "JFK"에서 시작하는 사람의 항공권이므로 "JFK"에서 시작해야 합니다. 유효한 여정이 여러 개인 경우, 어휘 순서가 작은 순서로 여정을 진행해야 합니다.

- 예를 들어, ["JFK", "LGA"]는 ["JFK", "JGB"]보다 어휘 순서가 작습니다.

모든 항공권은 적어도 하나의 유효한 여정을 구성한다고 가정합니다. 모든 티켓은 한 번만 사용할 수 있습니다.

## 해결 방법

어휘 순으로 방문해야하기 때문에 먼저, 정렬을 진행한 후 그래프로 구성한다. 구성한 그래프를 DFS 탐색을 활용하여 끝까지 탐색한다.

1. 입력 받은 `tickets`를 어휘순으로 정렬한다.
2. `Map`자료형에 `<from(string), to(string[])>`형태로 저장한다.
3. 첫 시작점인 `"JFK"`부터 `Map`에서 탐색하면서 `to` 배열의 첫 번째 요소를 꺼낸다.
4. 꺼낸 요소로 재귀 탐색을 진행하면 끝까지 연결 할 수 있다.

## 풀이 코드

```typescript
function findItinerary(tickets: string[][]): string[] {
  const answer: string[] = [];
  const graph = new Map<string, string[]>();

  tickets = tickets.sort();

  for (let [src, dest] of tickets) {
    if (!graph.has(src)) graph.set(src, []);
    graph.get(src).push(dest);
  }

  const dfs = (src: string) => {
    while (graph.get(src)?.length) {
      dfs(graph.get(src).shift());
    }
    answer.push(src);
  };

  dfs('JFK');
  return answer.reverse();
}
```

## 테스트 코드

```typescript
describe('332. Reconstruct Itinerary', () => {
  test('example test 1', () => {
    const output = findItinerary([
      ['MUC', 'LHR'],
      ['JFK', 'MUC'],
      ['SFO', 'SJC'],
      ['LHR', 'SFO'],
    ]);
    const expected = ['JFK', 'MUC', 'LHR', 'SFO', 'SJC'];
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const output = findItinerary([
      ['JFK', 'SFO'],
      ['JFK', 'ATL'],
      ['SFO', 'ATL'],
      ['ATL', 'JFK'],
      ['ATL', 'SFO'],
    ]);
    const expected = ['JFK', 'ATL', 'JFK', 'SFO', 'ATL', 'SFO'];
    expect(output).toEqual(expected);
  });
});
```
