---
layout: post
title: "마크다운 문법: 내부 링크와 특수문자"
description: "마크다운 문서 내부 링크가 동작하지 않으면 특수문자가 문제일 수 있습니다. 특수문자 제거, 미리보기, custom id로 해결해봅시다!"
date: 2021-08-05 14:06:00 +0900
last_modified_at: 2023-09-09 23:25:00 +0900
author: newticket-wonka
categories: [Language, Markdown]
tags: [link, curiosity]
---

> 블로그 글을 쓰고 나서 이전에 작성한 내부 링크가 동작하지 않는 것을 확인했다.
>
> 무엇이 문제였는지 알아보고자 한다.
>
> 이를 해결할 수 있는 몇 가지 방법도 알아보자.

## 페이지 내부 링크 사용법

![h1에서 h6에 해당하는 마크다운 링크가 현재 문서에서 작동한다.](/assets/img/21-08-05/test_link.gif "클릭 마다 해당하는 부분으로 이동하는 화면")

마크다운 문서 내부에서 링크로 이동하기 위해서는 아래와 같이 사용하면 된다.

```markdown
[보이는 텍스트](#이동-위치-텍스트)

...

# 이동 위치 텍스트
```

사용할 수 있는 헤더는 H1 ~ H6 까지로 마크다운에서 지원하는 헤더이다.

이 때, 이동할 위치의 텍스트 부분을 작성할 때는 2가지를 지켜야한다.

1. 영어는 **'소문자'** 만 가능

2. 띄어쓰기는 '-'로 변경

---

## 동작하지 않는 링크

링크가 동작하지 않는다면 먼저 **링크 클릭 시 연결되는 주소**와 문서에서 **이동할 주소**가 **같은지** 먼저 확인해보자.

![동일 문서의 하이퍼링크와 목표 위치의 주소가 다르다.](/assets/img/21-08-05/wrong_link_example.gif "마크다운 내부 링크가 동작하지 않는다.")

```markdown
[1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!](#1.-다른-장소에서-ec2-인스턴스에-연결하려니-안-돼요!)

...

## 1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!

...
```

소문자와 띄어쓰기를 적용했는데 링크가 다르게 나온다.

### 특수문자 문제?

```markdown
# 링크에 특수문자가 사라졌다.

.../#1-다른-장소에서-ec2-인스턴스에-연결하려니-안-돼요
```

다른 특수문자도 그런지 확인해보면 '-', '\_' 를 제외한 다른 특수문자는 지워진 것을 확인할 수 있다.

또한, 특수문자가 지워진 뒤에 링크가 겹치게 된다면 뒤에 '-숫자'를 붙이는 것으로 보인다.

![하이폰, 언더스코어 제외한 다른 특수문자는 주소가 변횐되어 표시된다.](/assets/img/21-08-05/special_characters_link.gif "일부 특수문자를 포함한 링크 동작 실험")

## 해결법

### 특수문자 안쓰기

이동할 링크에 '-', '\_'를 제외한 특수문자를 지워서 해결할 수 있다.

![하이퍼링크를 통해 동일 문서의 목표 위치로 이동한다.](/assets/img/21-08-05/working_link_example.gif "마크다운 내부 링크가 동작한다.")

```markdown
<!-- 수정 전 -->
<!-- [1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!](#1.-다른-장소에서-ec2-인스턴스에-연결하려니-안-돼요!) -->

<!-- 수정 후 -->

[1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!](#1-다른-장소에서-ec2-인스턴스에-연결하려니-안-돼요)

...

## 1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!

...
```

### Visual Studio Code의 Markdown Preview를 활용

![우측 상단 visual studio code의 markdown preview를 여는 버튼이 활성화되어있다.](/assets/img/21-08-05/vscode_markdown_preview.png "visual studio code의 open preview 버튼")

하지만 하나씩 특수문자를 지우는게 귀찮을 수 있다.

Visual Studio Code의 **Markdown: Open Preview**를 사용하면 내가 작성한 마크다운의 미리보기가 가능하다.

이를 통해 링크의 동작을 확인한다면 더욱 쉽게 확인할 수 있다.

또는 처음에 동작하지 않는 링크를 확인했을 때 **이동해야 할 링크를 클릭해서 주소를 파악**한 후 해당 링크를 사용하면 된다.

### Custom Heading ID 사용

![html header의 id가 text의 띄어쓰기와 특수문자가 변경된 형태이다.](/assets/img/21-08-05/html_heading_id.png "자동으로 채워진 header의 id")

개발자 도구를 통해 확인해보면 id 특성 값이 채워져 있다.

아래와 같이 id 특성 값을 변경하여 사용이 가능하다.

```markdown
[보이는 텍스트](#custom-id)

...

# 이동 위치 텍스트 {#custom-id}
```

몰론, [id 특성 값 이름 붙이기 규칙](https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/id)도 존재하고 마크다운의 헤더 규칙과 비슷하다.

#### MD051/link-fragments: Link fragments should be valid

![custom id를 적용하자 visual studio code에서 MD051 problem이 발생했다.](/assets/img/21-08-05/md051.png "링크에 경고가 발생")

[markdownlint의 MD051](https://github.com/DavidAnson/markdownlint/blob/main/doc/md051.md)은 링크와 헤더가 일치하지 않기 때문에 나오는 경고이다.

문서 안에 끊어진 링크를 찾는 데 도움을 주는 것이다.

그러니, 본인이 custom id를 사용하겠다면 이를 신경 쓰지 않고 사용해도 된다고 생각한다.

## 참조

- [Anchors in Markdown](https://gist.github.com/asabaylus/3071099)

- [ANCHORLINKS doesn't work for headers containing some special characters #160](https://github.com/sirthias/pegdown/issues/160)

- [Linking to Heading IDs](https://www.markdownguide.org/extended-syntax/#linking-to-heading-ids)