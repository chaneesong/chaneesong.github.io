---
title: Reorder Data in Log Files
author: chaneesong
date: 2023-02-12 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [937. Reorder Data in Log Files](https://leetcode.com/problems/reorder-data-in-log-files/description/)

## 문제 설명

log 배열이 주어진다. 각각의 로그는 공백으로 구분되어있고, 첫 번째 단어는 식별자다.
로그에는 두 가지 타입이 있다.

- Letter-logs: 모든 단어(식별자 제외)는 소문자로 구성되어 있다.
- Digit-logs: 모든 단어(식별자 제외)는 숫자로 구성되어 있다.

로그를 재정렬 하는 방법

- 문자 로그는 숫자 로그보다 앞에 온다.
- 문자 로그는 사전 순으로 정렬되어 있다. 내용이 같다면, 식별자 기준으로 정렬한다.
- 숫자 로그는 입력 순으로 정렬한다.

위 방법으로 재정렬 된 로그 배열을 리턴하라.

## 해결 방법

1. 입력 받은 로그 배열을 문자와 숫자 로그로 분리한다.
2. 문자 로그는 사전 순으로 재 정렬한다.
3. 문자 로그 뒤에 숫자 로그를 합친다.

## 풀이 코드

```typescript
function reorderLogFiles(logs: string[]): string[] {
  const stringLog: string[] = [];
  const numberLog: string[] = [];

  for (let log of logs) {
    const isString = isNaN(Number(log.split(" ")[1]));
    if (isString) stringLog.push(log);
    else numberLog.push(log);
  }

  stringLog.sort((a: string, b: string): number => {
    let aLog = a.substring(a.indexOf(" ") + 1);
    let bLog = b.substring(b.indexOf(" ") + 1);

    if (aLog !== bLog) return aLog.localeCompare(bLog);
    return a.localeCompare(b);
  });

  return stringLog.concat(numberLog);
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from "@jest/globals";
import { reorderLogFiles } from ".";

describe("reorder log files describe", () => {
  test("test1", () => {
    expect(
      reorderLogFiles([
        "dig1 8 1 5 1",
        "let1 art can",
        "dig2 3 6",
        "let2 own kit dig",
        "let3 art zero",
      ])
    ).toEqual([
      "let1 art can",
      "let3 art zero",
      "let2 own kit dig",
      "dig1 8 1 5 1",
      "dig2 3 6",
    ]);
  });

  test("test1", () => {
    expect(
      reorderLogFiles([
        "a1 9 2 3 1",
        "g1 act car",
        "zo4 4 7",
        "ab1 off key dog",
        "a8 act zoo",
      ])
    ).toEqual([
      "g1 act car",
      "a8 act zoo",
      "ab1 off key dog",
      "a1 9 2 3 1",
      "zo4 4 7",
    ]);
  });
});
```

## 구현 후

파이썬과 다르게 자바스크립트에서는 sort에서 상당히 애를 먹었다.  
사실 localeCompare의 존재도 몰랐고, 다중 조건으로 cb을 짜는 부분이 어려웠다.  
내가 알고리즘 문제를 푸는 이유도 알고리즘에 대한 이해도를 높이기 위함도 있지만,  
문제를 풀다 보면 언어의 특성에 대해 알 수 있어서다.
또, ts로 하면서 console로 테스트하지 않고 jest를 이용해 test코드를 짜보는 연습을 하고 있다.  
그런데 계속 하드 코딩으로 인자를 비교해야하는지 한 번 찾아볼 필요가 있겠다.
