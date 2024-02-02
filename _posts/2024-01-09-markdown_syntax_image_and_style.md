---
title: "마크다운 문법: 이미지와 스타일"
description: "이미지 작성을 위한 마크다운 문법과 이미지 스타일 문법이 없는 이유를 알아봅시다! 마크다운 확장 중에는 이미지 스타일을 다루는 버전이 있을까요?"
date: 2024-01-10 21:02:00 +0900
last_modified_at: 2024-01-18 03:28:00 +0900
author: newticket-wonka
categories: [Language, Markdown]
tags:
  [image, philosophy, curiosity]
---

> 블로그를 정리하면서 전에 쓴 글을 다시 읽었는데 이미지를 `<img>` 태그로 작성했습니다.
>
> 전에는 이미지의 크기와 정렬을 위해 HTML을 사용했지만, 지금은 크기가 중요하지 않아 `<img>`태그에서 마크다운 방식으로 변경하였습니다.
>
> "어떤 이유로 마크다운 문법으로 이미지의 형태를 정할 수 있게 하지 않았을까" 라는 궁금증에서 글을 쓰기 시작했습니다.

## 마크다운의 이미지 문법

먼저 마크다운에서 이미지를 사용하는 방법을 알아봅시다.
[Markdown: Syntax](https://daringfireball.net/projects/markdown/syntax#img)에서 말하길 마크다운에서 이미지 구문은 *인라인*과 *참조* 두 가지 방식이 있습니다.

### 인라인 방식

```markdown
![Alt text](/path/to/img.jpg)

![Alt text](/path/to/img.jpg "Optional title attribute")
```
{: .nolineno }

* `!`로 시작
* `[]`로 묶인 `alt` 속성
* `()`로 묶인 이미지 경로 또는 URL
* 선택적으로 `()` 안 이미지 경로 또는 URL 뒤에 `''` 또는 `""`로 묶인 `title`속성

### 참조 방식

```markdown
![Alt text][id]

...

[id]: url/to/image "Optional title attribute"
```
{: .nolineno }

참조 방식을 이용하면 문서 소스를 훨씬 쉽게 읽을 수 있다는 장점이 있습니다.
인라인 방식의 `!`와 `[]`로 묶인 `alt`속성까지는 같지만 다른 점은 다음과 같습니다.

* `[]`로 묶인 이미지 식별을 위한 `id` 
  * 문자, 숫자, 공백, 구두점으로 구성
  * 대소문자를 구분하지 않음
  * 선택적인 `공백`으로 두 `[]`를 구분
* 문서 어느 곳에서 `[]`로 묶인 `id`를 정의 
  * 선택적으로 `공백` 최대 3개를 왼쪽 여백에 들여쓰기
* `:` 뒤 하나 이상의 `공백` 또는 `탭`
* 이미지 경로 또는 URL 
  * 선택적으로 `<>`로 묶을 수 있음
* 선택적으로  `''` 또는 `""`로 묶인 `title`속성
  * URL의 가시성을 높이기 위해 `title`속성을 다음줄에 배치하고 왼쪽에 `공백` 또는 `탭`을 사용 가능

#### 암시적 방식

여기서 `id`를 지우고 *암시적인 방식*의 이미지 참조 또한 가능합니다.

```markdown
![Alt text][]

...

[Alt text]: url/to/image "Optional title attribute"
```
{: .nolineno }

* 이미지 식별을 위한 `id` 대신 `alt`속성을 통해 연결
  * `alt`속성이 공백을 포함하는 여러 단어라도 작동

이미지 참조 방식은 [마크다운 링크](https://daringfireball.net/projects/markdown/syntax#link)의 참조 방식과 동일하게 정의되어 있습니다.

### 이미지 크기는 어디에

마크다운 이미지 문법에는 이미지의 *크기*를 정하는 방식이 따로 정해져 있지 않습니다.
원한다면 HTML의 `<img>` 태그를 사용할 수 있다고 언급합니다.

마크다운에서 이미지의 *크기*를 위한 문법을 왜 따로 만들지 않았을까요? *형태*를 정하는 요소보다 중요한 것이 있었을까요?

## 마크다운의 철학

마크다운은 가능한 **읽고 쓰기 쉽도록** 만들어졌습니다. 즉, [마크다운의 철학](https://daringfireball.net/projects/markdown/syntax#philosophy)은 **가독성** 입니다.

여기서 말하는 *가독성*은 웹페이지에서 보이는 가독성이 아닌, *마크다운으로 작성된 문서 자체로 나타나는 가독성*입니다.
예를 들어, 지금 `F12`를 눌러 현재 페이지의 `html`요소 또는 마우스 우클릭으로 페이지 소스를 확인해봅시다.
같은 내용이지만 다양한 태그와 포맷 형식으로 묶여있어 문서 자체로는 내용을 파악하기 어렵습니다.

마크다운은 다양한 텍스트-HTML 변환기에서 영향을 받았지만, 가장 큰 영감의 원천은 줄 글로 이루어진 *이메일 형식*이라고 합니다.

### 마크다운과 HTML의 관계

따라서 마크다운의 시작은 HTML 태그를 더 쉽게 작성하도록 하는게 아니라, *문서 자체를 읽고 쓰기 쉽게 하는 것*입니다. 그렇기에 마크다운은 HTML을 대체하기위한 것이 아니고 문자열로 표시되는 극히 일부만이 대응되는 형식입니다.
[마크다운 문서](https://daringfireball.net/projects/markdown/syntax#html)에서는 HTML 태그의 사용을 제한하지 않고 필요할 경우 사용하라고 권장합니다.

이런 이유로 마크다운을 사용할 때 이미지 크기를 정하고 싶다면 **HTML 태그**를 사용하면 됩니다.

## 한계 돌파: 다양한 마크다운

이제 마크다운은 블로그, 깃허브, 위키 등 다양한 곳에서 사용됩니다.
하지만 이런 곳에서 마크다운을 사용할 때는 2004년 마지막 릴리즈인 [마크다운 1.0.1](https://daringfireball.net/projects/markdown/)을 그대로 사용하기보다 각자 다양하게 변경하거나 확장된 버전을 사용합니다.

예를 들어, 현재 글을 쓰는 [Github Pages는 기본적으로 Jekyll을 사용](https://docs.github.com/ko/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll)하여 웹 사이트를 빌드합니다.
그리고 [Jekyll의 마크다운 문서](https://jekyllrb.com/docs/configuration/markdown/)에서 말하길 Jekyll은 기본적으로 kramdown 프로세서를 사용한다고 나와있습니다.

위 [kramdown](https://kramdown.gettalong.org/index.html) 뿐 아니라 [GitHub Flavored Markdown (GFM)](https://github.github.com/gfm/)과 [CommonMark](https://commonmark.org/) 등 다양한 마크다운이 있습니다. 이미지 크기에 관해 GFM과 CommonMark는 이전 마크다운의 문법과 다르지 않지만 kramdown은 확장된 문법을 통해 이를 용이하게 합니다.

### kramdown의 이미지 크기 문법

마크다운과 이미지 문법은 같지만 [Inline Attribute Lists (IAL)](https://kramdown.gettalong.org/syntax.html#inline-attribute-lists)을 통해 속성(attribute)을 추가할 수 있습니다. 이를 활용해서 크기를 정할 수 있습니다.

```markdown
![Alt text](/path/to/img.jpg "Optional title attribute"){:height="36px" width="36px"}

![Alt text][id]

...

[id]: url/to/image "Optional title attribute"
{: height="36px" width="36px"}
```
{: file="kramdown}

## 결론

마크다운이 글을 읽고 쓰는 것을 쉽게 하도록 만들어졌기에 이미지 크기를 정하는 것은 목적과 조금 거리가 있습니다.
다만 이미지 크기처럼 마크다운에서 지원하지 않는 기능을 HTML을 통해 사용하도록 권장합니다.

부족한 표준과 기능의 확장을 위해 다양한 마크다운 버전이 생겼고 이 중 이미지 크기를 조절하는데 도움을 주는 마크다운 확장 버전도 있었습니다.

저는 잠시 '*마크다운을 쓰면서 HTML을 사용하는 것이 철학에 벗어나는 것이 아닐까?*' 하는 편협한 생각을 했습니다. 
하지만 결국 마크다운도 **사람이 쉽게 문서를 쓰기 위한 도구**고 문서에 나왔듯이 이 생각은 *그 의도에 맞지 않다*는 것을 알았습니다.

앞으로 어떤 표준, 규칙이라도 유연하게 생각할 수 있는 법을 기르고, 어떤 일에 있어 주객전도되지 않도록 노력해야겠습니다. 또 지금 내가 사용하는 도구가 무엇인지, 어떤 필요에 의해 만들어졌는지 잘 알아야겠다는 생각이 듭니다.

마지막으로 한 커뮤니티에서 [마크다운에서 이미지 스타일링을 위한 글](https://dzone.com/articles/how-to-style-images-with-markdown)을 찾았습니다.
CSS와 URL을 활용하는 방식이 소개되어 있습니다.
확장된 다른 마크다운을 사용할 수 없는 경우라면 고려해 볼 수 있을 듯 합니다.
다만, 링크된 글도 언급하는 내용이지만, '마크다운에 HTML은 정말 쓰기 싫어!'가 아니라면 HTML을 사용하는 것을 저는 추천합니다.
