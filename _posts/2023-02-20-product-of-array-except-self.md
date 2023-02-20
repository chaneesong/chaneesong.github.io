---
title:  Product of Array Except Self
author: chaneesong
date: 2023-02-20 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

## 문제 설명

정수 배열 `nums`가 주어질 때, `result[i]`는 `nums[i]`를 제외한 배열의 모든 숫자의 곱과 같게 하여 리턴하라.

최대 숫자는 32-bit 정수임을 보장한다.

시간 복잡도는 `O(n)`이내로 하고, 나눗셈을 사용할 수 없다.

## 해결 방법

`result`배열에 자기 자신을 기준으로 왼쪽과 오른쪽을 곱한다.

해당 문제는 `O(n)`의 시간복잡도를 가져야 하기 때문에 이전의 값을 이용하는 일종의 DP를 사용해야 한다.

1. `p`라는 포인터 변수를 하나 두고, 해당 변수에 `result`에 저장하고, 오른쪽으로 곱해나간다.
2. `p`변수를 1로 다시 초기화 하고 오른쪽에서 시작하여 `result`에 곱하고, 왼쪽으로 곱해나간다.

테스트케이스의 동작 과정은 아래와 같다.

![movement](/assets/img/algorithm/product-of-array-except-self/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
export function productExceptSelf(nums: number[]): number[] {
  const result: number[] = [];
  let p = 1;

  for (let i = 0; i < nums.length; i++) {
    result.push(p);
    p = p * nums[i];
  }

  p = 1;
  for (let i = nums.length - 1; i > 0 - 1; i--) {
    result[i] = result[i] * p;
    p = p * nums[i];
  }

  return result;
}
```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { productExceptSelf } from '.';

describe(' description', () => {
  test('example test 1', () => {
    const output = productExceptSelf([1, 2, 3, 4]);
    const expected = [24, 12, 8, 6];
    expect(output).toEqual(expected);
  });

  test('example test 2', () => {
    const output = productExceptSelf([-1, 1, 0, -3, 3]);
    const expected = [0, 0, 9, 0, 0];

    output.forEach((num, i) => expect(num === expected[i]).toEqual(true));
  });
});
```

## 구현 후

해당 문제는 생각하는게 어려웠지 막상 코드로 짜기는 쉬웠다.  
그런데 예상치도 못하게 테스트케이스에서 말썽을 부렸다.  
jest를 사용해서 자료구조 내부 값을 비교할 때 보통 `toEqual`을 많이 사용했다.  
그런데 두 번째 테스트케이스에서 곱해서 0으로 만드는 경우가 있다.  
자바스크립트에서는 음수 * 0 예로 들면, `-5 * 0 = -0`이다.  
이게 그냥 동등 비교할 때에는 `true`로 나오지만, `toEqual`에서는 다르다고 나온다.  
때문에 테스트케이스를 작성 할 때 꽤나 애를 먹었다.  
여러 가지 방법이 있지만 결국 가장 가독성이 좋은 동등 비교를 사용해서 테스트케이스를 작성했다.

추가로, 해당 문제는 꽤나 오래 jest issue에 등록되어 있을 정도로 알려진 문제이므로 고쳐지기 전까지는 기억하고 있는 것이 좋겠다.  

## [Jest Issue link](https://github.com/facebook/jest/issues/12221)
