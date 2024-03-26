---
title: "Java - int <-> string"
date: 2024-03-11 23:15:00 +0900
author: newticket-wonka
categories: [Lauguage, Java]
tags: [java]
---

[_Java™ Platform Standard Ed. 8_](https://docs.oracle.com/javase/8/docs/api/)

## Int to String

### String = int + ""

String `+`연산자는 javac에서 구현되고, 컴파일 시 내부적으로는 `StringBuilder`로 만든 뒤 문자열로 반환하기 때문에 사용하지 않는게 좋음

```java
String banana = "바나나";
banana += "쥬스";
// 위 아래 동일
String bananaJuice = new StringBuilder("바나나").append("쥬스").toString();
```

계속해서 `StringBuilder`객체가 생성되고 `append()`, `toString()` 메서드가 호출되기 때문에 성능이 저하되고 메모리 낭비가 커진다.

따라서 변경이 많다면 처음부터 `StringBuilder` 클래스로 만들어 문자열을 합치는게 더 좋은 방법

### valueOf와 toString

#### String.valueOf(int i)

![String value of](/assets/img/24-03-11/string-value-of.png)

#### null?

`String.valueOf(null)`을 실행하면 `"null"` 문자열이 출력된다는 글이 많다.
`String` 클래스의 `valueOf`의 파라미터로는 `Object`, `char[]`, `boolean`, `cha[]]`, `int`, `long`, `float`, `double`이 있다.
이 중에 `Object`와 `char[]`를 제외하고는 해당 클래스의 `toString` 메소드를 사용한다.

```java
/**
     * Returns the string representation of the {@code Object} argument.
     *
     * @param   obj   an {@code Object}.
     * @return  if the argument is {@code null}, then a string equal to
     *          {@code "null"}; otherwise, the value of
     *          {@code obj.toString()} is returned.
     * @see     java.lang.Object#toString()
     */
    public static String valueOf(Object obj) {
        return (obj == null) ? "null" : obj.toString();
    }
```

다만 `Object`의 경우에는 `null` 문자열을 반환한다.

#### Integer.toString(int i)

![Integer to String](/assets/img/24-03-11/integer-to-string.png)

### String.Format(String format, Object)

```java
String str = "";
int number = 123;

str = String.Format("%d", n);
System.out.print(Number);
// 123
```

## String to int

```java
String str = "17";

int num1 = Integer.parseInt(str);
System.out.println(num1); // 17

Integer num2 = Integer.valueOf(str);
System.out.println(num2); // 17
```

### Integer.parseInt

기본 자료형(Primitive Type)인 `int`로 반환

### Integer.valueOf

참조 자료형(Reference Type)인 `Integer`객체로 반환

#### Auto Unboxing & Auto Boxing

`int num2 = Integer.valueOf(str);`로 사용해도 괜찮다.

이는 자동 언박싱이라 부르는데 JDK 1.5부터 자바 컴파일러가 자동으로 처리해주기 시작했다.

```java
Integer num = 17;
int n = num;
```

그래도 내부적으로는 추가 연산 작업을 거치기에 동일한 타입 연산을 하도록 구현하는 편이 좋다.
