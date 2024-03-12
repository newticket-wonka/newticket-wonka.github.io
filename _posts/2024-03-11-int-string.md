---
title: "Java - String to int"
date: 2024-03-11 23:15:00 +0900
author: newticket-wonka
categories: [Lauguage, Java]
hidden: true
tags: [java]
---

[_Java™ Platform Standard Ed. 8_](https://docs.oracle.com/javase/8/docs/api/)

## String = int + "";

String `+`연산자는 javac에서 구현되고, 컴파일 시 내부적으로는 `StringBuilder`로 만든 뒤 문자열로 반환하기 때문에 사용하지 않는게 좋음

```java
String banana = "바나나";
banana += "쥬스";
// 위 아래 동일
String bananaJuice = new StringBuilder("바나나").append("쥬스").toString();
```

계속해서 `StringBuilder`객체가 생성되고 `append()`, `toString()` 메서드가 호출되기 때문에 성능이 저하되고 메모리 낭비가 커진다.

따라서 변경이 많다면 처음부터 `StringBuilder` 클래스로 만들어 문자열을 합치는게 더 좋은 방법

## String.valueOf(int i)

![String value of](/assets/img/24-03-11/string-value-of.png)
`Integer.toString`랑 친해보인다.

숫자 문자열로 바꾸는 방법

## Integer.ToString(int i)

![String to String](/assets/img/24-03-11/string-to-string.png)
String 본인 그대로 반환하는것도 있다.

## String.Format(String format, Object)

```java
String str = "";
int number = 123;

str = String.Format("%d", n);
System.out.print(Number);
// 123
```
