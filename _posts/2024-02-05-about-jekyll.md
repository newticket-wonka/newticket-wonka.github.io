---
layout: post
author: newticket-wonka
title: "Jekyll에 대해서"
description: "Jekyll이 왜 만들어졌는지, 어떻게 쓰는건지 알아봅시다."
date: 2024-02-05 13:28:00 +0900
categories: [DEV, Jekyll]
tags: [
    blog,
    web,
    ruby,
    jekyll
]
---

> 블로그 홈에 TIL글이 너무 많아서 사이드바를 만들고 싶어졌습니다.
> Jekyll을 어떻게 쓰는지 알아야 수정도 할 수 있으니 알아보려고 합니다.

## 배경

[Jekyll](https://jekyllrb.com/)은 Github의 공동 설립자 [Tom Preston-Werner](https://en.wikipedia.org/wiki/Tom_Preston-Werner)가 2008년 출시했습니다.
[본인이 jekyll을 출시하며 낸 글](https://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html)에는 당시 WordPress나 Mephisto같은 복잡한 블로그 엔진, 수많은 템플릿 페이지의 스타일 지정, 댓글 조정, 최신 소프트웨어 릴리스에 뒤처짐 등 글 작성 외적인 작업을 하기 싫고 *좋은 글을 쓰고싶었다*고 합니다.
글과 사이트는 최대한 편하게 작성, 관리하며 디자인도 쉽게 맞춤설정 할 수 있길 원했습니다.

## Jekyll은 뭐지?

[공식 문서](https://jekyllrb.com/docs/)에서 말하길 Jekyll은 정적 사이트 생성기(Static Site Generator)로 마크업 언어로 쓰인 글을 가지고 레이아웃을 이용해 정적 웹사이트를 만드는 프로그램(ruby gem)입니다.

### 정적 웹사이트 그리고 동적 웹사이트

그렇다면 여기서 정적(Static) 웹사이트는 무엇일까요?
동적(Dynamic) 웹사이트도 있을까요?

정적 웹사이트는 서버에 저장된 파일 그대로 사용자에게 제공되는 웹페이지입니다.
여기에는 HTML, CSS, JavaScript가 포함됩니다.

동적 웹사이트는 크게 Client-Side Rendering(CSR)과 Server-Side Rendering(SSR)로 나눌 수 있습니다.

Client-Side Rendering은 먼저 거의 비어있는 HTML, CSS, JavaScript 파일을 사용자에게 보내고 사용자의 JavaScript에서 필요한 다른 CSS, JavaScript 파일, 데이터를 서버와 통신을 통해 가져옵니다.

Server-Side Rendering은 사용자의 요청에 따라 서버에서 HTML, CSS, JavaScript 파일을 생성해서 보냅니다.

> 저는 처음에 '정적 웹사이트면 JavaScript를 안쓰고 그대로 보여주는거 아닌가?' 라고 생각했습니다.
> 하지만 정적 웹사이트도 JavaScript를 사용할 수 있습니다.
> 예를 들어 입력만 받고 바로 JavaScript로 html에 출력, 현재 시간 출력, 주사위 굴리기 등이 있습니다.
{: .prompt-tip}

### Gem?

Gem은 객체지향 언어인 ruby로 작성된 프로그램(라이브러리, 코드 모음)입니다.
Gem을 설치하기 위해 RubyGems를 사용합니다.
RubyGems는 ruby의 패키지 관리 시스템으로 `gem` 명령어를 사용해 ruby gem을 검색, 설치, 배포, 관리합니다.

Gem을 위한 gem도 있습니다.
Bundler는 gem의 의존성 관리 도구로 Gemfile에 있는 ruby gem 목록을 읽고 설치한 뒤 정확한 버전 정보를 담은 Gemfile.lock을 생성하거나 업데이트합니다.
이후 실행할 때는 Gemfile.lock을 먼저 확인하고 명시된 gem 버전을 기반으로 설치합니다.

이렇게 bundler를 사용하면 프로젝트의 환경을 일관되게 유지하고 의존성 충돌을 방지할 수 있습니다.

> 우리가 로컬에서 사이트를 실행하기 위해 사용하는 `bundle exec jekyll serve`는 bundler에게 지금 프로젝트 폴더에 있는 gemfile을 보고 jekyll을 실행하라는 명령입니다.
> `bundle exec`를 안붙이고 `jekyll serve`만 사용할 수 있습니다.
> 대신 시스템 전역에 설치된 jekyll 버전이 사용될 수 있으니 프로젝트의 의존성을 제어하는데 어려울 수 있습니다.
{: .prompt-info}

## Jekyll의 [철학](https://jekyllrb.com/philosophy/)

1. 마법은 없다.
    * 많이 읽지 않아도 흐름이 어떻게 되는지 알 수 있어야 한다.
    * 요구한 것 만 수행한다
    * 결과에 집중하고 그것이 쉽게 이해 가능하도록 해야한다.
2. 그냥 된다.
    * 오류 때문에 사이트가 작동하지 않는 것보다 즉시 사용 가능해야한다.
3. 컨텐츠가 왕이다.
    * 사용자가 컨텐츠를 즐겁고 간편하게 관리할 수 있어야 한다.
4. 안정성
    * 이전 버전과 높은 호환성에 중점을 두고 사이트 구축에 문제가 없도록 신경써야한다.
    * 불가피하게 변경사항이 발생하면 업그레이드 할 수 있는 명확한 경로를 제공해야한다.
5. 소형 & 확장성
    * Jekyll의 기본 코어는 90%의 사용자가 사용하는 기능을 포함하는 아주 단순하고 작은 것이여야 한다.
    * 확장 기능은 플러그인으로 제공되야한다.

## 연습 해보기

[튜토리얼](https://jekyllrb.com/docs/step-by-step/01-setup/)을 보고 [프로젝트의 구조](https://jekyllrb.com/docs/structure/), jekyll이 사용하는 템플릿 언어 [Liquid](https://shopify.github.io/liquid/)와 확장된 [필터](https://jekyllrb.com/docs/liquid/filters/), [태그](https://jekyllrb.com/docs/liquid/tags/)를 참조하면 필요한 디자인을 만들 수 있을 것입니다.

> [해석 순서](https://jekyllrb.com/docs/liquid/tags/)와 [렌더링 단계](https://jekyllrb.com/docs/rendering-process/)에 대한 자세한 정보
{: .prompt-info}
