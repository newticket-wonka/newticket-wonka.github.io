---
layout: post
title: "Visual Studio Code에서 저장 시 자동 줄바꿈 - Prettier와 Markdown"
description: "VSCode에서 파일를 저장할 때 자동으로 줄이 바뀐다면 포맷 설정을 확인해봅시다. Prettier을 사용한다면 더더욱"
date: 2024-02-18 18:28:00 +0900
author: newticket-wonka
categories: [DEV, Visual Studio Code]
tags: [ide, format]
---
> Jekyll 블로그 테마인 Chirpy에는 코드 블럭을 작성할 때 해당 파일 이름을 붙일 수 있는 [기능](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/_posts/2019-08-08-text-and-typography.md?plain=1#L124)이 있습니다.
>
> ![파일를 저장하기 전 코드는 한 줄 씩 써있다.](/assets/img/24-02-18/before_save_markdown.png '저장 전에는 코드 블럭과 IAL 부분이 붙어있다.')
> *저장하기 전*
>
> ![파일가 저장되자 코드 중간 공백이 생긴다.](/assets/img/24-02-18/after_save_markdown.png '저장 후 코드 블럭과 IAL 부분이 떨어졌다.')
> *저장한 후*
>
> [이전 글](https://newticket-wonka.github.io/posts/TIL-24-02-17/)을 작성할 때 코드 블럭에 파일 이름을 붙이기 위해 `{: file="pyport.h"}`을 다음 줄에 작성했는데 저장하자 분리되었습니다.

## Prettier

VSC에서 formatter로 [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)를 사용하고 있습니다.
Prettier의 깃허브 저장소에 [관련 이슈](https://github.com/prettier/prettier/issues/10128)가 나온 적 있습니다.
그런데 우선순위에 밀려난 듯 보입니다.

## VSC 설정

`F1` 또는 `Ctrl`+`Shift`+`P`로 명령 팔레트를 열어 `preferences settings json`을 검색해 적용을 원하는 부분의 `JSON` 파일을 엽니다.
여기서는 `사용자 설정 열기`로 열었습니다.

적당한 부분에 아래 부분을 작성합니다.

```json
...
"[markdown]": {
    "editor.formatOnSave": false
  },
...
```
{: file='settings.json'}

이 설정은 VSC 설정(`Ctrl`+`,`)에서 `Editor: format on save` 항목을 마크다운에서는 해제합니다.
