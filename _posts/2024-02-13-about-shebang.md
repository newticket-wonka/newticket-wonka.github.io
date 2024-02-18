---
layout: post
author: newticket-wonka
title: "셔뱅(#!, Shebang)에 대해서"
description: "셔뱅(#!, Shebang)이 무엇인지, 어떻게 쓰는건지 알아봅시다."
date: 2024-02-13 16:42:00 +0900
categories: [DEV, Linux]
tags: [shebang, magic number, mac, linux, script]
---

> python을 다시 공부하던 중 `#!/usr/bin/python3`를 보고 익숙하지 않아 찾아보게 되었다.

## 셔뱅이란

- `#!`로 표기하는 문자 시퀀스
- 스크립트의 맨 처음에 온다.
- 유닉스 계열 OS에서 이 기호가 있으면 나머지 부분을 [interpreter directive](https://en.wikipedia.org/wiki/Interpreter_directive)로 구문 분석
  - 해당 경로에 있는 인터프리터 프로그램을 실행
  - 실행한 스크립트의 경로를 해당 프로그램에게 넘긴다.

## 예시

```python
#!/usr/bin/python3
print('hi')
```
{: file='test'}

```bash
$ python3 test
hi
$ ./test
hi
```
{: .nolineno}

셔뱅이 없으면 기본 쉘로 실행한다.

```shell
echo hi
```
{: file="test"}

```sh
$ ./test
hi
```
{: .nolineno}

## 매직 넘버

[매직 넘버](<https://en.wikipedia.org/wiki/Magic_number_(programming)#In_files>)는 컴퓨터 프로그래밍에서 다른 의미도 갖지만 유닉스, 리눅스 계열에서 파일 유형 메타데이터를 통합하기 위해서 파일 내부 첫 2 bytes 구간에 매직 넘버를 할당하고 저장한다.

셔뱅은 매직 넘버의 특별한 경우로 실행하려는 파일(스크립트)을 어떤 인터프리터로 읽을지 결정한다.
