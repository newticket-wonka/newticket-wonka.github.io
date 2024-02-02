---
layout: post
author: newticket-wonka
title: "zip으로 다운받은 Chirpy 테마 업그레이드"
description: "Zip으로 다운받은 Chripy 테마를 업그레이드 하는 과정과 방법을 알아봅시다!"
date: 2023-09-10 22:27:00 +0900
last_modified_at: 2023-09-24 14:06:00 +0900
categories: [Blog]
tags: [git, chirpy, upgrade, problem]
---

> 지난 내부 링크 글을 작성하면서 페이지의 오른쪽 목차(Table Of Content)에 헤딩이 표시되지 않는 것을 확인했다.
>
> ![h4가 목차에 표시되지 않음](/assets/img/23-09-10/h4_no_toc.png "페이지 요소와 목차")
> 본인 말고도 많은 사람들이 이 기능을 [언급](https://github.com/cotes2020/jekyll-theme-chirpy/issues/1023)했고 최근 업그레이드에서 해당 사항이 반영되었다.
>
> Chirpy의 [업그레이드 문서](https://github.com/cotes2020/jekyll-theme-chirpy/wiki/Upgrade-Guide)에서 충돌이 일어날 수 있고 이를 침착하게 해결하라 조언하니 어떤 문제가 생길지 아주 기대가 된다.
>
> starter를 사용했다면 블로그를 생성했다면 위 문서를 참고해 업그레이드 하시면 됩니다!
>
> 저는 다른 테마를 사용하다가 테마를 chirpy로 바꾸어서 zip파일을 통해 업그레이드 해보았습니다.

## 환경

| OS | Windows |
| 블로그 생성 방법 | zip 파일 다운로드 |

## 계획

__우선 Visual Studio Code에서 `.git` 폴더를 제외하고 모든 파일을 덮어쓴 뒤 달라진 부분을 Source Control에서 확인하는 방식이 간단합니다.__

저는 전에 다른 테마를 사용하다가 chirpy의 zip 파일을 그대로 가져다 넣어서 혹시 남아있을 전 테마 파일들을 삭제하는 겸 git 연습도 해보며 zip 파일로 진행해보겠습니다.

새 버전 zip파일을 다운받은 후 -> Chirpy 테마의 블로그 작업을 한 적 없는 커밋으로 돌아가 새 버전의 브랜치를 만들고 -> `main` 브랜치와 병합 후 생기는 충돌을 하나씩 침착하게 수정해보겠습니다.

## 최신 버전 다운로드

![chirpy 블로그 테마의 다양한 버전이 있다.](/assets/img/23-09-10/chirpy-latest-version-download.png "chirpy 테마 릴리즈 목록")

Chirpy의 업그레이드할 [최신 버전](https://github.com/cotes2020/jekyll-theme-chirpy/tags)을 찾아 다운로드 합니다.

이 시점에는 v6.2.2의 zip파일을 다운로드 하겠습니다.

## Conflict 만들기

```bash
# 이전 커밋으로 이동
# git checkout [커밋 번호]
git checkout 7bef3d

# 브랜치 생성, 변경
git branch test
git checkout test

# .git을 제외한 기존 파일 전부 삭제
rm -rf *
rm -r .commitlintrc.json .gitattributes .gitmodules .prettierrc .editorconfig .github/ .husky/ .stylelintrc.json .browserslistrc .gitignore .nojekyll .versionrc.json

# 다운로드한 chirpy zip파일 압축해제
unzip /d/Download/jekyll-theme-chirpy-6.2.2.zip
cd jekyll-theme-chirpy-6.2.2/

# 보이는 파일과 숨겨진 파일 모두 꺼내오기
mv * ../
mv .* ../

# 압축했던 디렉토리 삭제
rm -r jekyll-theme-chirpy-6.2.2/

# 전부 커밋
git add --all
git commit -m 'Chirpy 6.2.2'

# main 브랜치 이동 후 병합
git checkout main
git merge test
```

이제 충돌이 난 부분을 하나 씩 확인하며 수정합니다.

### 브랜치 이동하지 마십쇼!

git을 사용해 보신 분이라면 당연히 아시겠지만 본인은 심히 초보라 이걸 몰라 __저장소 몇번 날려먹길__ 반복했습죠...

위 과정에서 `test` 브랜치를 만들고 파일들을 다 압축해제한 뒤 커밋하지 않고 `main` 브랜치로 이동한다면 새 파일들이 공유되고 굉장히 귀찮아집니다.

만약 저처럼 저장소를 밀어버리고 다시 만들겠다 하신다면 다른 작업을 다 하신 뒤

저장소에서 `Setting - Pages - Build and deployment`에서 Source를 Github Actions로 변경 & 아래 `Jekyll by github actions`를 Configure를 통해 Commit changes까지 진행하셔야 합니다.

> Github Action과 Jekyll Configure를 하지 않으면 index.html인 `--- layout: home # index page ---`만 주구장창 보게 되실겁니다!
{: .prompt-warning}

## JS 파일 컴파일 (v5.6.0 부터)

저는 기능적인 부분은 변경사항이 없어서 별다른 충돌이 많이 나오지 않았습니다.

_config.yml과 일부 포스팅 파일을 수정해주고 계속해 봅시다.

Chirpy의 업그레이드 문서에 따르면 2023년 5월 19일 나온 `v5.6.0` 이후 부터는 JS 파일 컴파일을 직접 해주어야 한다고 합니다.

```bash
npm run build
```

### Error: Cannot find module '@rollup/plugin-babel'

`Ok to proceed? (y)`에 `y`를 몇번 누르고 나니 에러가 나왔습니다.

```bash
...
[!] Error: Cannot find module '@rollup/plugin-babel'
Require stack:
...
```

`@rollup/plugin-babel` 모듈이 없다고 하니 설치해 줍시다.

```bash
npm install @rollup/plugin-babel --save-dev
```

설치 후에 다시 `npm run build`를 통해 js파일들을 컴파일하고 `git add assets/js/dist -f`로 저장소에 추가합시다.

나중에 다시 업그레이드할 때를 대비해 `.gitignore`의 `assets/js/dist` 부분을 주석 처리해도 괜찮을 것입니다.

## 결과

![h4가 목차에 표시됨](/assets/img/23-09-10/h4_toc.png "페이지 요소와 목차")

오른쪽 목차에 h4의 헤딩이 추가되어 나오는걸 확인할 수 있습니다. 와! 업그레이드 성공!

> 저장소를 새로파고 커밋이 반영되지 않는다면 [Source가 Github Action인지 & Jekyll Configure가 되었나](#브랜치-이동하지-마십쇼) 봅시다!
{: .prompt-tip}

## 참조

[[Commit] 과거 커밋으로 돌아가서 새로운 브랜치(분기) 만들기](https://gobae.tistory.com/142)

[unzip(1) - Linux man page](https://linux.die.net/man/1/unzip)

[mv(1) - Linux man page](https://linux.die.net/man/1/mv)

[@rollup/plugin-babel](https://www.npmjs.com/package/@rollup/plugin-babel)

[git 에서 commit을 안하고 브랜치를 이동한다면?](https://engineer-diary.tistory.com/60)

[Jekyll Chirpy(v6.0.1) 테마를 활용한 Github 블로그 만들기(2023.6 기준)](https://jjikin.com/posts/Jekyll-Chirpy-%ED%85%8C%EB%A7%88%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%9C-Github-%EB%B8%94%EB%A1%9C%EA%B7%B8-%EB%A7%8C%EB%93%A4%EA%B8%B0(2023-6%EC%9B%94-%EA%B8%B0%EC%A4%80)/)