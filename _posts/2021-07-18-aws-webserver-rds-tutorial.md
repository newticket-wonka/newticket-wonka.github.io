---
layout: post
title: "자습서: 웹 서버 및 Amazon RDS DB 인스턴스 생성 수행록"
description: "자습서: 웹 서버 및 Amazon RDS DB 인스턴스 생성을 통해 겪은 쉘, DNS 연결 등 여러 문제와 해결법을 확인해보세요!"
date: 2021-07-18 19:46:00 +0900
last_modified_at: 2023-09-09 21:41:00 +0900
author: newticket-wonka
categories: [DEV, AWS]
tags:
  [ tutorial, 
    amazon rds, 
    amazon ec2,
    linux,
    public dns,
    ami,
    database,
    problem
  ]
---

> [자습서: 웹 서버 및 Amazon RDS DB 인스턴스 생성](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/TUT_WebAppWithRDS.html)
>
> 위 자습서를 수행하면서 본인이 마주친 문제점 및 팁을 정리한 글입니다.
>
> 자세한 수행 방법은 자습서를 참고해주세요!

## 알아두기

해당 튜토리얼은 진행하면서 **과금이 발생**합니다.

예제 실행 후, 생성한 EC2 인스턴스, RDS DB 인스턴스를 중지 말고 **삭제**해야 이후 과금되지 않을 것입니다!

### 환경

| OS | Windows |
| SSH Client | PuTTY |

### 결과

![웹 페이지에서 이름, 주소 정보를 추가한다. 해당 정보를 페이지에서 확인 가능하다.](/assets/img/21-07-18/ezgif.com-gif-maker.gif "자습서 최종 결과")

웹 페이지에서 입력한 정보가 데이터베이스에 저장된 것을 확인할 수 있습니다!

## 질문 목록

1. [다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!](#1-다른-장소에서-ec2-인스턴스에-연결하려니-안-돼요)

2. [윈도우에서 텍스트 복사를 했는데 리눅스 쉘 또는 nano에 붙여넣기를 어떻게 하나요?](#2-윈도우에서-텍스트-복사를-했는데-리눅스-쉘-또는-nano에-붙여넣기를-어떻게-하나요)

3. [PuTTY에서 퍼블릭 DNS 연결할 때 _my-instance-user-name@my-instance-public-dns-name_ 를 어떻게 작성해야 하나요?](#3-putty에서-퍼블릭-dns-연결할-때-my-instance-user-namemy-instance-public-dns-name-를-어떻게-작성해야-하나요)

4. [nano에서 저장/종료를 어떻게 하나요?](#4-nano에서-저장종료를-어떻게-하나요)

5. [EC2 인스턴스 중지 후 다시 시작했는데 PuTTY 또는 웹 페이지가 연결이 안 돼요!](#5-ec2-인스턴스-중지-후-다시-시작했는데-putty-또는-웹-페이지가-연결이-안-돼요)

6. [로컬에 있는 파일을 EC2 인스턴스로 가져오려면 어떻게 해야하나요?](#6-로컬에-있는-파일을-ec2-인스턴스로-가져오려면-어떻게-해야하나요)

7. [RDS DB 인스턴스의 데이터를 어떻게 확인할 수 있나요?](#7-rds-db-인스턴스의-데이터를-어떻게-확인할-수-있나요)

8. [데이터베이스에 PROCEDURE, TRIGGER, FUNCTION을 생성하려니 에러가 납니다!](#8-데이터베이스에-procedure-trigger-function을-생성하려니-에러가-납니다)

### 1. 다른 장소에서 EC2 인스턴스에 연결하려니 안 돼요!

-> EC2 인스턴스의 보안그룹(tutorial-securitygroup) 인바운드 규칙의 SSH유형에 해당하는 소스를 수정해주세요!

튜토리얼의 [퍼블릭 웹 서버에 대해 VPC 보안 그룹 생성](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html#CHAP_Tutorials.WebServerDB.CreateVPC.SecurityGroupEC2)의 _4 - a_ 항목을 확인해보면 <https://checkip.amazonaws.com> 에서 확인한 퍼블릭 IP를 사용합니다.

### 2. 윈도우에서 텍스트 복사를 했는데 리눅스 쉘 또는 nano에 붙여넣기를 어떻게 하나요?

-> **Shift + Insert**로 붙여넣기 합니다!

### 3. PuTTY에서 퍼블릭 DNS 연결할 때 _my-instance-user-name@my-instance-public-dns-name_ 를 어떻게 작성해야 하나요?

-> **ec2-user@_EC2인스턴스 퍼블릭 IPv4 DNS_** 형식으로 작성합니다!

튜토리얼에서는 Amazon Linux 2 AMI를 사용하기 때문에 ec2-user를 사용합니다.

다른 경우에는 [인스턴스를 시작하는데 사용한 AMI의 기본 사용자 이름](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/connect-to-linux-instance.html#w679aac19c21b7c17b7b5b4b3b3)을 참고하여 작성할 수 있습니다.

### 4. nano에서 저장/종료를 어떻게 하나요?

-> 저장 : **Ctrl + O** / 종료 : **Ctru + X**

### 5. EC2 인스턴스 중지 후 다시 시작했는데 PuTTY 또는 웹 페이지가 연결이 안 돼요!

-> 다시 시작하면 퍼블릭 IPv4 DNS가 변경됩니다!

PuTTY - Configuration - Session 에서 HostName의 @뒤를 새로 작성해주세요.

### 6. 로컬에 있는 파일을 EC2 인스턴스로 가져오려면 어떻게 해야하나요?

-> 아래 명령을 통해 전송합니다!

```bash
scp -i [pem파일경로] [업로드할 파일 이름] [ec2-user계정명]@[ec2 instance의 public DNS]:~/[경로]

# ex)
scp -i ".../~.pem" test.txt ec2-user@ec2~~~~~~.ap-northeast-2.compute.amazonaws.com:~/test/
```

### 7. RDS DB 인스턴스의 데이터를 어떻게 확인할 수 있나요?

-> 아래 명령을 통해 데이터베이스에 접속합니다!

```bash
mysql -u 사용자-이름 -p -h RDS-DB-엔드포인트

ex)
mysql -u tutorial-user -p -h tutorial-db-instance.~~~.rds.amazonaws.com

```

이후 마스터 암호를 입력하고 접속합니다.

만약 암호를 잊으셨다면 RDS DB 인스턴스에서 **수정**을 통해 새 마스터 사용자 암호를 입력할 수 있습니다.

### 8. 데이터베이스에 PROCEDURE, TRIGGER, FUNCTION을 생성하려니 에러가 납니다!

-> DB 인스턴스의 파라미터 그룹에서 **log_bin_trust_function_creators** 파라미터를 **true**로 설정해야 합니다!

[AWS re:Post 지식 센터의 글](https://aws.amazon.com/ko/premiumsupport/knowledge-center/rds-mysql-functions/)을 참고하여 활성화할 수 있습니다.

## 참조

- [자습서: 웹 서버 및 Amazon RDS DB 인스턴스 생성](https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/UserGuide/TUT_WebAppWithRDS.html)

- [PuTTY 란 무엇입니까? PuTTY 다운로드, 복사 및 붙여 넣기하는 방법](https://kogoza.tistory.com/entry/PuTTY-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9E%85%EB%8B%88%EA%B9%8C-PuTTY-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EB%B3%B5%EC%82%AC-%EB%B0%8F-%EB%B6%99%EC%97%AC-%EB%84%A3%EA%B8%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)

- [Nano 단축키](https://zetawiki.com/wiki/Nano_%EB%8B%A8%EC%B6%95%ED%82%A4)

- [[AWS] EC2 ssh 원격 접속과 scp를 통한 파일 전송](https://ict-nroo.tistory.com/40)

- [Amazon RDS DB 인스턴스의 마스터 사용자 암호를 재설정하려면 어떻게 해야 하나요?](https://aws.amazon.com/ko/premiumsupport/knowledge-center/reset-master-user-password-rds/)
