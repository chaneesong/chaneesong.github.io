---
title: Reverse String
author: chaneesong
date: 2023-02-11 +0900
categories: [leetcode]
tags: [알고리즘]
render_with_liquid: false
---

### [344. Reverse String](https://leetcode.com/problems/reverse-string)

## 문제 설명

문자열을 뒤집는 함수를 작성하라.  
추가적인 메모리 사용없이 리스트를 직접 조작하여야 한다.

## 해결 방법

리스트의 절반을 순회하여 앞과 뒤를 바꾼다.

![reverse-string](/assets/img/reverse-string.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function reverseString(s: string[]): void {
  for (let i = 0; i < Math.floor(s.length / 2); i++) {
    const tmp = s[i];
    s[i] = s[s.length - 1 - i];
    s[s.length - 1 - i] = tmp;
  }
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from "@jest/globals";
import { reverseString } from ".";

describe("Compare Reverse String", () => {
  test("hello", () => {
    const s: string[] = ["h", "e", "l", "l", "o"];
    reverseString(s);
    expect(s).toEqual(["o", "l", "l", "e", "h"]);
  });

  test("Hannah", () => {
    const s: string[] = ["H", "a", "n", "n", "a", "h"];
    reverseString(s);
    expect(s).toEqual(["h", "a", "n", "n", "a", "H"]);
  });
});
```

## 느낀점

해당 문제는 사실 while문에 투 포인터를 활용하라는 문제로 알고 있다.  
그렇게 했을 때 가독성이 더 좋아보이는 것도 사실이지만, for문을 사용해도 가독성을 크게 해치지는 않는 것 같다.  
또, typescript로 풀면서 테스트코드를 작성해보고 있는데, jest에서 toBe와 toEqual의 차이가 뭔지 알게 되었다.  
~~사실 이렇게 테스트코드를 작성하는 건지는 잘 모르겠다.~~

### [Github 풀이코드](https://github.com/chaneesong/algorithm/tree/main/typescript/String-Manipulation/reverse-string)
