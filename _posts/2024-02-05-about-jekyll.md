---
layout: post
author: newticket-wonka
title: "Jekyll은 무엇인가 - 사이드바 항목 추가하기 (미완성)"
description: "Jekyll이 왜 만들어졌는지, 어떻게 쓰는건지 그리고 공부한 내용으로 사이드바를 수정까지 해봅시다."
date: 2024-02-05 13:28:00 +0900
last_modified_at: 2024-02-05 02:58:00 +0900
categories: [DEV, Jekyll]
tags: [
    blog,
    web,
    jekyll,
    chirpy
]
---

> 블로그 홈에 TIL글이 너무 많아서 사이드바를 만들고 싶어졌습니다.
> Jekyll을 어떻게 쓰는지 알아야 수정도 할 수 있으니 알아보려고 합니다.

## Jekyll이 뭐지?

[공식 문서](https://jekyllrb.com/docs/)에서 말하길 Jekyll은 정적 사이트 생성기(Static Site Generator)로 마크업 언어로 쓰인 글을 가지고 레이아웃을 이용해 정적 웹사이트를 만듭니다.

### 정적 웹사이트 그리고 동적인 다른것

그렇다면 여기서 정적(Static) 웹사이트는 무엇일까요?
동적(Dynamic) 웹사이트도 있을까요?

정확하진 않으나 공부한 내용으로 구별해보면 다음과 같습니다.

우선, 가장 큰 차이점은 *서버와의 통신이 있냐 없냐*입니다.

정적 웹사이트는 서버에 저장된 파일 그대로 사용자에게 제공되는 웹페이지입니다.
여기에는 HTML, CSS, JavaScript가 포함됩니다.

동적 웹사이트는 크게 Client-Side Rendering(CSR)과 Server-Side Rendering(SSR) 두 방식으로 나눌 수 있습니다.

Client-Side Rendering은 먼저 거의 비어있는 HTML, CSS, JavaScript 파일을 사용자에게 보내고 사용자의 JavaScript에서 필요한 다른 CSS, JavaScript 파일, 데이터를 서버와 통신을 통해 가져옵니다.

Server-Side Rendering은 사용자의 요청에 따라 서버에서 HTML, CSS, JavaScript 파일을 생성해서 보냅니다.

저는 처음에 '정적 웹사이트면 JavaScript를 안쓰고 그대로 보여주는거 아닌가?' 라고 생각했습니다.
하지만 정적 웹사이트도 JavaScript를 사용할 수 있습니다.
예를 들어 입력만 받고 바로 JavaScript로 html에 출력, 현재 시간 출력, 주사위 굴리기 등이 있습니다.
