---
title: Most Common Word
author: chaneesong
date: 2023-02-13 +0900
categories: [알고리즘]
tags: [leetcode]
render_with_liquid: false
---

### [819. Most Common Word](https://leetcode.com/problems/most-common-word)

## 문제 설명

문자열로 된 문장과 금지어 목록이 배열로 주어질 때, 금지어가 아니면서 가장 많이 등장하는 단어를 리턴하라.  
적어도 한 단어는 금지어로 지정되어 있지 않고, 정답은 하나 뿐이다.  

문장의 단어들은 대소문자가 구분되어 있지 않고, 정답은 소문자로 반환되어야 한다.

## 해결 방법

1. 주어진 문장을 배열로 파싱한다.
    1. 주어진 문장을 소문자로 바꾼다.
    2. 알파벳이 아닌 문자를 공백으로 변환한다.
    3. 공백을 기준으로 문장을 분리한다.
2. 단어의 숫자를 카운트 하기 위해 배열을 순회하며 object 자료형에 [word: count]형태로 저장한다.
    * 저장할 때 금지어는 생략한다.
3. object의 key 값을 추출하여 가장 많이 등장한 단어를 찾는다.

![parsing](/assets/img/most-common-word.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

## 풀이 코드

```typescript
export function mostCommonWord(paragraph: string, banned: string[]): string {
  const words = paragraph.toLowerCase().replace(/[^\w]/g, ' ').split(' ');
  let wordsObj = {};
  let mostWord = '';
  let mostCount = 0;

  for (let word of words) {
    if (word.length === 0 || banned.includes(word)) continue;
    if (!wordsObj[word]) wordsObj[word] = 0;
    wordsObj[word]++;
  }

  Object.keys(wordsObj).forEach((word) => {
    if (mostCount < wordsObj[word]) {
      mostWord = word;
      mostCount = wordsObj[word];
    }
  });

  return mostWord;
}

```

## 테스트 코드

```typescript
import { describe, expect, test } from '@jest/globals';
import { mostCommonWord } from '.';

describe(' description', () => {
  test(' test description', () => {
    const value = mostCommonWord(
      'Bob hit a ball, the hit BALL flew far after it was hit.',
      ['hit']
    );
    expect(value).toEqual('ball');
  });

  test(' test description', () => {
    const value = mostCommonWord('a.', []);
    expect(value).toEqual('a');
  });
});

```

## 구현 후

더 최적화 할 방법이 있어보이는 코드이다.  
하지만 충분히 빠르고, 쉬워보이기도 하니 넘어가도록 하자(Hint: 반복문 하나로 처리).  

파싱에서 긴가민가하는 부분이 있어서 ts 사용 최초로 디버깅을 시도했는데,  
node환경에서는 ts 디버깅을 바로 지원하지 않고 js로 트랜스컴파일(빌드) 후 디버깅을 하는 것 같다.  
그런데 테스트를 간편하게 하기 위해서 루트 디렉토리에 tsconfig와 jest.config.js 파일을 두었더니,  
루트부터 전체가 트랜스파일 되는 현상이 있었다.  
특정 디렉토리만 타겟으로 트랜스컴파일을 하려고 하니 뭔가 어렵다.  
이게 문제가 되는 이유는, 디버깅 후 찌꺼기 파일들을 다 삭제해야하는데 너무 귀찮다.  
find에 rm 명령어를 써도 되긴 하겠지만(옛날에 한 번 root에서 썻다가 포맷했던 적이 있어서 그런가 무서웠다.),  
일단은 outDir옵션을 사용해 따로 한곳에 모아둔 뒤 삭제하기로 했다.  
일단, 특정 디렉토리만 빌드하는 것은 나중에 찾아보기로 하고, 넘어가자.
