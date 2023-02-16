---
title: vscode에서 lazygit 사용하기
author: chaneesong
date: 2023-02-16 +0900
categories: [editor]
tags: [vscode]
render_with_liquid: false
---

## Intro

요즘 vscode를 설정하는데 푹 빠져있다.  
원래 더 자유도가 높고, 마우스도 잘 안쓰는 neovim을 쓰려고 했는데, 컴퓨터가 안좋은 탓인지 몰라도 커맨드를 여러 개 중첩시키면 제대로 동작하지 않는 현상이 벌어진다.  
예로 들면, 전체 선택을 하기 위해 커서를 파일 끝으로 옮기고, 선택 모드로 진입 후 첫 라인으로 이동, 그리고 커서를 앞으로 옮기는 커맨드를 만들었는데, 제대로 동작하지 않았다. 아마 딜레이를 주고 어쩌고 하다보면 될 테지만, 한국어 자료도 별로 없는 neovim을 잘 쓸 자신도 없고, 익숙하지 않아서 그런가 생산성도 떨어지는 기분이 들었다.  
해서 vscode로 다시 넘어왔다.  

그런데 이거 보세요...

![vscode-git](/assets/img/vscode-lazygit/vscode-git.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

이게 2023년도에 git을 지원하는 에디터 맞습니까? ㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋㅋ  
물론 여러가지 확장을 쓰게 되면, 진짜 좋아지긴 하지만 단 한가지 커밋 작성에 대해서는 정말 꽝이다.  
이래저래 찾아봐도 뭐가 없었고, 내심 인텔리제이에서 사용하는 코드 단위 커밋은 정말 써보고 싶은데 유료라서 사용할 엄두가 안 난다.  
그래도 뭐 깃을 쓸 때는 소스트리나 lazygit을 쓰면 되니깐 문제가 없는데, 이걸 켜고 끄는 것 마저 귀찮아지기 시작했다.  
그래서 몇날을 고민한 끝에 '아 내가 그냥 vscode 확장을 만들어야겠다.' 생각했다.  

## 고민

먼저, 소스트리는 자체 실행 파일이기 때문에 제외를 했고, 'lazygit을 어떻게 하면 vscode에서 바로 실행되게 할 수 있을까?'를 고민했다.  
내가 원하는 것은 vscode는 하단에 자체 터미널 탭을 가지고 있기 때문에 이걸 최대화 시킨 후, lazygit 명령어를 실행시키는 것이다.  
근데 중요한 점은 내가 extension을 한 번도 만들어 본 적이 없어서 이걸 만드는데 얼마나 걸릴지 알 수가 없다는 것이다.  

그래서 뭐 여차저차 구글 검색을 때려본 결과 이미 [나와 같은 생각](https://aishmn.medium.com/use-lazygit-with-vscode-task-and-keyboard-shortcut-2b548e7336ad)을 하고 있는 사람을 찾았다.  
해당 블로그 글은 vscode task를 만들어서 lazygit을 띄우는 명령어를 만드는 것인데,,,  
여기서 문제는 저 명령어를 실행시키면 터미널에 포커스를 두는 것과 터미널 창을 최대화 시키는 것은 또 따로 해야하는 것이다.  
나는 하나의 단축키로 터미널 포커스, 최대화, lazygit 실행을 하고 싶은데 말이지... 그러다가 우연히 [진짜 대박인 확장](https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command)을 찾았다.  
간략하게 설명하자면 커맨드 여러 개를 한 루틴에 묶을 수 있는 확장인데, **내가 딱 원하던 거다!**  

## 해결

와 이 확장을 찾은건 정말 천운이나 다름없다.  
이게 진짜 대박인게 이걸 잘만 사용하면 **vscode를 vim처럼 사용**할 수 있다.  
일단 추후에 그렇게 만들도록하고, 내가 지금 원하는 걸 해야지.  

해당 확장으로 다중 커맨드를 사용하는 법은 간단하다.  
먼저 `settings.json`에 사용할 커맨드를 등록하고, `keybindings.json`에서 바인딩만 해주면 끝이다.  

코드로 보면 아래와 같다.

`settings.json`

```json
[
  "multiCommand.commands": [
    {
      "command": "startLazygit",
      "sequence": [
        "workbench.action.toggleMaximizedPanel",
        "terminal.focus",
        {
          "command": "workbench.action.terminal.sendSequence",
          "args": { "text": "lazygit\n", "delay": 100 }
        }
      ]
    }
  ]
]
```

`keybindings.json`

```json
[
  {
    "key": "cmd+ctrl+l",
    "command": "extension.multiCommand.execute",
    "args": {
        "command": "startLazygit" 
    },
  }
]
```

먼저 `settings.json`에 사용할 command를 등록해주고, sequence에 내가 원하는 vscode command를 순차적으로 작성해주고, `keybindings.json`에서 저렇게 등록해주기만 하면 잘 실행된다.  

여기서 **주의할 점**은 기존 바인딩 할 때 커맨드를 커스텀한 커맨드로 작성하는게 아니고, 기존 커맨드는 `extension.multiCommand.execute`를 넣어주고, 인자로 커스텀 커맨드를 넣어주는 것이다.

나는 `cmd+ctrl+l`에 매핑했고, 실행하고 나서 지렸다.

![before](/assets/img/vscode-lazygit/before.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

실행 하기 전에 이 화면이었다면, 실행하게 되면...

![after](/assets/img/vscode-lazygit/after.png){:style='border:1px solid #eaeaea; border-radius: 7px; padding: 0px;' }

### Awesome

미쳐버리겠다. 내 일주일 동안의 고민을 한 번에 해결해주신 개발자 [ryuta46](https://github.com/ryuta46)님 정말 감사드립니다!!!

## 후기

아직 저 커맨드는 엉뚱한 작동을 할 가능성이 높다. 그럼에도 너무 흥분한 나머지 이 확장을 전파하고자 해당 게시글을 작성했다.  
엉뚱한 작동은 현재 터미널이 다른 디렉토리에 있어도 현재 워크 스페이스가 아닌 다른 위치에서 lazygit이 실행 될 것이다.  

뭐, 그 정도는 다른 터미널 세션을 띄우는 작업을 넣는다던지, 현재 위치로 가도록 하는 커맨드를 넣는다던지 해서 쉽게 해결할 수 있으니깐...  
이 확장을 잘만 사용하면 여러 커맨드를 잘 중첩시켜 진짜 대박인 커맨드를 만들어낼 수 있겠다.
