---
title: vscode에서 typescript 디버깅
author: chaneesong
date: 2023-02-14 +0900
categories: [typescript]
tags: [debugging]
render_with_liquid: false
---

## INTRO

vscode에서 타입스크립트를 디버깅하는 방법은 인터넷에도 많이 나와있고, vscode 공식문서에도 나와있다.
그래서 생략하고, 나머지 내가 난관에 봉착했던 것들을 기록하려한다.

### [vscode 공식문서](https://code.visualstudio.com/docs/typescript/typescript-debugging)

## 문제 상황

알고리즘 문제를 풀던 도중 긴가민가하는 문제가 생겨서 디버깅을 해야하는 상황이 생겼다.  
그래서 위의 문서에 나와있는대로 디버깅을 하려하니 빌드는 되는데 실행이 잘 안된다.  
검색을 해보니 task파일을 만들어야 한다더라... 어케 찾은건지;;  
[참고 블로그](https://m.blog.naver.com/remocon33/222037614711)

그러고 나서 실행해보니 잘 되긴 되는데, 한 가지 문제가 생겼다.  
나는 따로 푼 문제들을 언어별로 관리를 하는데, typescript에서는 최초로 테스트코드를 도입해보려는 시도를 했다.  
그래서 아무 생각없이 루트 디렉토리에 tsconfig와 jest.config.js를 생성했는데,  
이렇게 하고 디버깅을 하려고 보니 모든 디렉토리에 빌드 파일이 생기는 것이다...  

![file](/assets/img/typescript-debugging-in-vscode/file.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

사실 이렇게 빌드가 되던 말던 상관없었다.  
find와 rm을 조합 한 후 다 찾아서 지워버리면 그만이다.  
그런데 일주일 전이었나 mysql을 항상 도커에서 쓰다가 로컬에서 쓰려니 비밀번호를 까먹었다.  
어차피 로컬에 저장된 데이터도 없을 뿐더러 잘 사용하지 않으니깐 밀었다가 다시 설치하려고 무려 root에서!!!  
`find / -name mysql -type d -exec rm {} \;`를 사용했다(왠만하면 쓰지맙시다.).  
그런데 예상치 못한 node에 mysql 디렉터리가 있었던 것이다.  
한 번 find 해보고 할걸...  
결국 에러를 해결 못하고, 포맷했는데 그 때 이후로 rm은 되도록 자제하려고 한다.  

## 해결 방법

애초에 타겟만 빌드되도록 하는 것이 좋아보여서 이것 저것 만져봤는데 도통 감이 안잡혔다.  
결국 어떻게 하는 지 몰라서 그냥 tsconfig에 빌드 목적지 두고 빌드 파일을 따로 한 곳에 모아놓은 뒤 나중에 끝나면 삭제하는 식으로 진행했다.  

## 마무리

아래와 같은 디렉토리 구조를 가지고 있다고 할 때,  
`most-common-word/index.ts`를 디버깅 하고 싶다면 아래와 같이 설정했다.

```markdown
algorithm
├─ .vscode
├─ javascript
├─ python
└─ typescript
   ├─ String-Manipulation
   │  ├─ most-common-word
   │  │  ├─ index.test.ts
   │  │  └─ index.ts
   │  ├─ reorder-data-in-log-files
   │  │  ├─ index.test.ts
   │  │  └─ index.ts
   ├─ jest.config.js
   ├─ tsconfig.json
```

```json
// tsconfig.json
{
    "compilerOptions":{
        "target": "ES2019",
        "sourceMap": true,
        "rootDir": "./",
        // 빌드 시 생성되는 파일 위치를 지정한다. 시작 위치는 tsconfig.json이 위치한 디렉토리
        "outDir": "dist"
    }
}
```

```json
// .vscode/launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "skipFiles": [
                "<node_internals>/**"
            ],
            // 타겟의 위치를 입력한다. workspaceFolder는 .vscode가 있는 디렉토리다.
            "program": "${workspaceFolder}/typescript/String-Manipulation/most-common-word/index.ts",
            "outFiles": [
                // 여기에 빌드 된 파일의 위치를 지정한다.
                "${workspaceFolder}/typescript/dist/**/*.js"
            ],
            "preLaunchTask": "tsc: build - typescript/tsconfig.json"
        }
    ]
}
```

```json
// .vscode/tasks.json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "typescript",
            "tsconfig": "typescript/tsconfig.json",
            "group": "build",
            "label": "tsc: build - typescript/tsconfig.json"
        }
    ]
}
```

분명히 tsconfig를 어떻게 만지면 내가 원하는 디렉토리만 빌드 할 수 있을 것 같은데, 추후 알아보자.
