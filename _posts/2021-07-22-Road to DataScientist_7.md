---
layout: post
title:  "Road to DataScientist - 5.Python 기초문법_3_Function"
date:   2021-07-22 
categories: DataScience 
tags: DataScience python 
---
# Python 기초문법_3
---

## 1. Function이란?

* *input*이 들어와서 *output*이 정해진 규칙을 통해 생성되는 수학에서의 function과 비슷합니다.
* 하지만 수학에서의 function과는 달리 프로그래밍에서는 하나의 기능을 나타내기도 합니다.
* 정확하게는 특정 기능을 구현한 *코드 묶음* 입니다.
* 함수를 사용하는 이유는 *재사용성* 때문 입니다.

>함수를 사용하는 가장 큰 이유는 재사용성 때문 입니다. 똑같은 구조의 코드가 반복되는 것을 피하기 위해서 사용되며 보통 한가지의 기능을 단위로 묶입니다.

## 2. Function definition

```python
def add(a, b):
    c = a + b
    return c
```

* 위와 같은 구조를 가지고 있습니다.

```python
add(3, 8)
```

* 위와 같은 방식으로 호출하며 인자(parameter)를 설정 할 수 있습니다.

* 만약 parameter의 갯수를 모를 경우 * 를 이용하여 함수를 정의 할 수 있습니다.

```python
def add_many(*args):
    return sum(args)
```

* parameter는 함수 내부에서 사용되는 local parameter 와 함수 밖에서도 통용되는 global parameter를 구분해서 사용합니다.
* 즉 함수 내부에서 사용된 parameter는 함수 외부로는 아무런 영향을 끼치지 못하며 함수 외부의 변수와 서로 충돌하지 않습니다.

```python
def change_name(name):
    name = "kim"
    print("name is changed by ", name)
    return name

name = "lee"
change_name(name)
print(name)
```

## 3. Lambda함수

* Lambda 함수는 한줄로 함수를 표현 할 수 있는 구문입니다.

```python
x = lambda a, b: a + b
print(x)
```

* 위와 같이 lambda를 선언 한 이후 바로 인자를 받으며 : 이후에는 return값을 결정합니다.
* 조금 더 기능을 확장하여 다음 리스트를 문자열의 길이로 정렬해 보겠습니다.

```python
L = ["jeong", "kim", "park", "namgung"]
L.sort(key = lambda x: len(x))

print(L)
```

---
# 마치며

설명한 부분 이외에도 공부할 부분은 많습니다만 간략하게 function에 대하여 설명해 보았습니다. 이 외에도 라이브러리를 불러와 내장된 함수를 사용하는 등 함수의 사용법은 훨씬 많이 있지만 여기선 간략하게 넘기고 다음에는 I/O에 대하여 설명해 보도록 하겠습니다.
