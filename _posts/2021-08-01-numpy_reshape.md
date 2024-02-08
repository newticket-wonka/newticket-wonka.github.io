---
layout: post
title: "NumPy.reshape에서 -1은 뭘까?"
description: "Python NumPy의 reshape 함수를 어떻게 사용하고, 입력으로 주어지는 -1이 어떻게 쓰이는지 알아봅시다!"
date: 2021-08-01 19:11:00 +0900
author: newticket-wonka
categories: [Language, Python]
tags: [numpy, curiosity]
---

> 학교에서 진행한 파이썬 데이터 분석 강의에서 아래 코드를 봤다.
>
> ```python
> [ In ]
> x = np.arange(10).reshape(-1, 1)
> y = (2*x + 1).reshape(-1, 1)
> x
> [ Out ]
> array([[0], [1], [2], [3], [4], [5], [6], [7], [8], [9]])
> [ In ]
> y
> [ Out ]
> array([[ 1], [ 3], [ 5], [ 7], [ 9], [11], [13], [15], [17], [19]])
> ```
>
> 여기서 **-1**의 역할이 뭔지 알고싶어서 찾아보게 되었다.

## numpy.reshape란?

NumPy 문서에 나온 numpy.reshape의 설명은 다음과 같다.

> numpy.reshape(a, newshape, order='C')
>
> Gives a new shape to an array without changing its data.

어떻게 사용하는지 알아보자.

```python
[ In ]
import numpy as np
a = [1,2,3,4,5,6]
b = np.reshape(a,(2,3))
c = np.reshape(b,(6))
b
[ Out ]
array([[1, 2, 3], [4, 5, 6]])
[ In ]
c
[ Out ]
array([1, 2, 3, 4, 5, 6])
```

## 그래서 '-1'은?

'-1'의 동작을 알기 위해 먼저 문서를 확인해보자.
여기서 궁금한건 *newshape*이다.

> Parameters : **newshape** : int or tuple of ints
>
> The new shape should be compatible with the original shape.
> If an integer, then the result will be a 1-D array of that length.
> **One shape dimension can be -1**.
> In this case, **the value is inferred from the length of the array and remaining dimensions**.

'-1'은 **_원래 배열의 길이와 남은 차원으로부터 추정_** 된다고 한다.

경우를 나눠 어떻게 동작하는지 확인해보자

## reshape(-1)인 경우

```python
[ In ]
a = np.arange(10).reshape(-1)
a
[ Out ]
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

`reshape(-1)`은 1차원 배열을 반환한다.

## reshape(-1, ?)인 경우

```python
[ In ]
a = np.arange(10).reshape(-1, 2)
a
[ Out ]
array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
```

열에 특정 값을 주면 그 수에 맞는 행의 수를 추정한다.

## reshape(?, -1)인 경우

```python
[ In ]
a = np.arange(10).reshape(2, -1)
a
[ Out ]
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
```

행에 특정 값을 주면 그 수에 맞는 열의 수를 추정한다.

## 이건 어때

### reshape(-1, -1)

```python
[ In ]
a = np.arange(10).reshape(2, -1)
a
[ Out ]
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-14-93acda1a9a22> in <module>()
----> 1 a = np.arange(10).reshape(-1, -1)
      2 a

ValueError: can only specify one unknown dimension
```

ValueError가 난다. 내용을 살펴보면 추정할 수 있는 차원은 하나만 지정할 수 있다고 한다.

2개 이상을 추정하라고 하면 정보가 부족하니 에러가 나는 것으로 보인다.

---

### 나눠지지 않는 수?

```python
[ In ]
a = np.arange(10).reshape(3, -1)
a
[ Out ]
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-17-46a7f0c2ca6c> in <module>()
----> 1 a = np.arange(10).reshape(3, -1)
      2 a

ValueError: cannot reshape array of size 10 into shape (3,newaxis)
```

ValueError가 난다. 사실, 이 경우는 -1이 아니더라도 나눠지지 않는 수로 정하면 이렇게 된다.

## 참조

- [NumPy - numpy.reshape](https://numpy.org/doc/stable/reference/generated/numpy.reshape.html)
