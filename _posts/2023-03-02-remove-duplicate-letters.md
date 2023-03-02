---
title: Remove Duplicate Letters
author: chaneesong
date: 2023-03-02 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## 문제 설명

문자열 `s`가 주어질 때, 모든 문자가 한 번씩만 나타나도록 중복 문자를 제거하라. 가능한 모든 결과 가운데 사전식 순서에서 가장 빠른 결과를 확인해야한다.

## 해결 방법

스택과 `Map`, `Set`을 적절히 활용하여 해결한다.

- 스택은 결과를 저장하기 위해서 사용  
- Map(Counter)은 문자의 등장 횟수를 파악하기 위해 사용  
- Set(Seen)은 문자가 이미 처리되었는지 판단하기 위해 사용

1. Map에 `문자: 등장 횟수`로 저장한다.
2. 문자열을 순회하면서 문자에 해당하는 `Counter value`를 1씩 감소시킨다.
3. 만약, Seen에 문자가 존재한다면, 이미 처리된 문자로 간주하고 건너뛴다.
4. 처리해야 될 문자라면, 스택의 문자와 사전 순서를 비교하여 `pop`한다. 처리 방법은 아래와 같다.
    - 스택의 top 문자가 뒤에 등장하면서(Counter에 값이 0보다 클 때) 현재 문자보다 순서가 뒤에 위치하는 경우
5. 스택을 처리한 뒤, 현재 문자를 스택에 `push`하고, `Seen`에 추가한다.

![movement](/assets/img/algorithm/stack/remove-duplicate-letters/movement.gif){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
function removeDuplicateLetters(s: string): string {
  const counter = new Map<string, number>();
  const seen = new Set<string>();
  const stack: string[] = [];

  for (let char of s) {
    if (!counter.has(char)) counter.set(char, 0);
    counter.set(char, counter.get(char) + 1);
  }

  for (let char of s) {
    counter.set(char, counter.get(char) - 1);
    if (seen.has(char)) continue;

    while (
      stack.length &&
      char < stack.at(-1) &&
      counter.get(stack.at(-1)) > 0
    ) {
      seen.delete(stack.pop());
    }
    stack.push(char);
    seen.add(char);
  }

  return stack.join('');
}

```

## 테스트 코드

```typescript
describe(' description', () => {
  test('example test 1', () => {
    const output = removeDuplicateLetters('bcabc');
    const expected = 'abc';
    expect(output).toBe(expected);
  });

  test('example test 2', () => {
    const output = removeDuplicateLetters('cbacdcbc');
    const expected = 'acdb';
    expect(output).toBe(expected);
  });
});
```

## 구현 후

object는 위와 같이 입력이 변할 때 어떤 식으로 타입을 정의해야할지 난감한데, Map과 Set은 제네릭을 이용하여 쉽게 타입을 정의할 수 있어서 편했다. 왠만하면 object 대신 Map이나 Set을 적극적으로 활용하는게 좋을 것 같다.
