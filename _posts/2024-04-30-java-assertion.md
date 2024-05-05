---
title: "Java - Assertion(검증)"
date: 2024-04-30 21:40:00 +0900
author: newticket-wonka
categories: [Language, Java]
tags: [java]
---

> Java에서 프로그램 실행 중 오작동 하거나 비정상적으로 종료되는 경우에 대응하기 위해 다양한 방법을 사용할 수 있다.
>
> [`assert`문](https://docs.oracle.com/javase/specs/jls/se8/html/jls-14.html#jls-14.10)은 `try-catch`에서 `throw exception`과 비슷해 보이지만 사용하는 목적이 다르다.

## assert문 형식

```java
assert Expression1;
assert Expression1: Expression2;

// Ex
assert money >=0 : "money는 음수가 될 수 없다~";
```

* `Expression1` - 조건 (거짓이면 `AssertionError` 발생)
* `Expression2` - 값, `AssertionError` 인스턴스에 담길 메시지 (`void` 메서드 X)

사실 동작만 보면 `if`문으로 조건을 걸고 `throw exception`하는 예외처리와 비슷해 보인다.

하지만 `assert`는 JVM 기본 설정에서는 실행 시 모두 제외된다.
`-ea`(`-enableassertions`) 옵션을 사용해서 실행해야 `assert`이 동작한다.
IDE에서는 Run Configurations에서 VM arguments에 추가해 실행한다.

> C++에서는 디버그 모드에서만 컴파일되어 실행되고 릴리즈 모드에서는 컴파일러가 `assert`를 무시하고 제거한다.

## assert의 목적

`assert`는 코드 실행 시 해당 조건이 참이라는 가정 하에 진행된다는 것을 명시하는 것이다.
코드 실행 중 발생하는 로직의 문제라기보단 개발 과정에서 이 조건을 상정하는 것이다.

## assert 사용하기

[`Assertion`을 사용하기 좋은 상황](https://docs.oracle.com/javase/8/docs/technotes/guides/language/assert.html#usage)의 예시이다.

* 내부 불변성 (Internal Invariants)
* 제어 흐름 불변성 (Control-Flow Invariants)
* 사전 조건, 사후 조건, 클래스 불변성 (Preconditions, Postconditions, Class Invariants)

### 내부 불변성(Internal Invariants)

프로그램 과정 중 여기서 이 값을 가정한다는 것을 표현한다.

어떤 수를 3으로 나눈 뒤 나머지에 따라 다른 동작을 실행하는 예시를 들 수 있다.

```java
if (i % 3 == 0) {
    ...
} else if (i % 3 == 1) {
    ...
} else {
    // i % 3 == 2 인 상황
}
```

코드를 작성할 때 의도는 `else`에서 *나머지가 2인 상황*을 가정하는 것이다.
여기서 `assert`를 사용할 수 있다.

```java
if (i % 3 == 0) {
    ...
} else if (i % 3 == 1) {
    ...
} else {
    assert i % 3 == 2 : i;
}
```

위 코드가 실행되기 전 `i`가 음수였다면 `AssertionError`가 발생할 것이다.
디버깅하는 과정에서 `AssertionError`를 확인하고 `i`가 음수인 입력 상황을 미리 발견하고 수정할 수 있다.

가정을 검증하는 과정에서 예외 상황을 놓치지 않고 견고한 코드를 작성하는데 도움이 된다.

### 제어 흐름 불변성(Control-Flow Invariants)

프로그램 과정 중 도달할 수 없는 곳에 `assert`를 사용한다.

```java
void method() {
    ...
    if(...)
        return;

    assert false;
}
```

사실 이런 경우는 유의해서 사용하지 않는다면 대체로 컴파일 에러가 발생할 것이다.

프로그래머가 가정한 경우의 흐름을 벗어나는 경우에 `assert`를 사용하는 방식도 있다.
예시로 트럼프 카드의 문양에 따라 다른 동작을 한다고 해보자.

```java
switch(suit) {
    case Suit.CLUBS:
        ...
        break;

    case Suit.DIAMONDS:
        ...
        break;

    case Suit.HEARTS:
        ...
        break;

    case Suit.SPADES:
        ...
        break;
    default:
        assert false : suit;
}
```

트럼프 카드의 문양 4가지 이외의 경우에 도달하지 않게 하기 위해 `assert`를 사용할 수 있다.
이 과정에서 문양이 4종류인 것을 가정하는 것이라 내부 불변성과도 이어지는 내용이다.

### 사전 조건, 사후 조건, 클래스 불변성 (Preconditions, Postconditions, Class Invariants)

* 사전 조건     : 메서드 호출 시 참이여야 하는 상황
* 사후 조건     : 메서드 실행 후 침이여야 하는 상황
* 클래스 불변성 : 각 클래스의 인스턴스의 상태에 대해 참이여야 하는 상황

공간을 예약하는 메서드를 예시로 사전 조건, 사후 조건, 클래스 불변성의 예시를 들 수 있다.

```java
private Booking makeBooking(LocalDateTime startTime, LocalDateTime endTime) {
    // 사전 조건: 시작 시간이 종료 시간보다 앞서야 한다.
    assert startTime.isBefore(endTime) : "시작 시간이 종료 시간보다 뒤에 있습니다.";

    Booking newBooking = new Booking(startTime, endTime);
    bookings.add(newBooking);
    // 클래스 불변성: bookings 리스트는 null이 아니어야 한다.
    assert bookings != null : "bookings 리스트가 null입니다.";

    // 사후 조건: 새로운 예약이 bookings 리스트에 추가되었는지 확인
    assert bookings.contains(newBooking) : "예약이 정상적으로 추가되지 않았습니다.";

    // 사후 조건: 예약 시간이 겹치지 않는지 확인
    for (Booking booking : bookings) {
        if (booking != newBooking && booking.overlaps(newBooking)) {
            assert false : "새로운 예약과 기존 예약 시간이 겹칩니다.";
        }
    }

    return newBooking;
}
```

## assert 주의사항

1. public 메서드의 인자에 `assert`를 사용하지 말 것
2. 프로그램 동작에 필수인 작업을 `assert`에 넣지 말 것

큰 이유로는 두 경우 모두 `assert`가 비활성화 될 수 있기 때문이다.

