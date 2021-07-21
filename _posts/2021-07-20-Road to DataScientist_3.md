---
layout: post
title:  "Road to DataScientist - 3.Python 기초문법_1_datatype"
date:   2021-07-20 
categories: DataScience python 
---
# Python 기초문법_1
---



# 1. DataType&Variable

시작하기에 앞서 이번 예제는 jupyter notebook을 이용해 실행하였습니다.

## 1.1 DataType이란?

Datatype은 컴퓨터가 data를 다루기 위해 data 종류별로 구분하여 저장하는 방식입니다.

Variable(변수)에 data를 저장하고 관리하며 이 variable라는 개념을 통해 data를 사용 할 수 있습니다.

datatype에는 int, float, str, char, list, tuple, dict, set ... 등 다양하게 있으며 주로 쓰이는 datatype 위주로 알아보려고 합니다.

## 1.2 Variable이란?

Variable(변수)란 메모리에 특정 데이터를 저장하기 위한 공간이며 이 공간에 이름을 지어줄 수 있습니다.

사용자, 즉 우리는 이 이름으로 변수를 구분하며 컴퓨터는 실제로 메모리를 차지하고 있는 주소를 이용하여 구분합니다.

즉 복잡한 문자와 숫자의 조합으로 된 주소를 사용자가 구분하기 쉽게 하기 위해 별명을 지어주는 것과 같습니다.

이러한 변수에 데이터를 넣어 주는 방법은 assignment(할당) 이라고 하는데 *=* 이라는 연산자를 사용합니다.

    a = 10
    
a 라고 이름지어진 변수에 10이라는 숫자 데이터를 넣어 준 것 입니다.

    id(a)
    
를 통해 실제로 변수 a가 저장된 주소를 알 수도 있습니다.

---

# 2.DataType


## 2.1 숫자형 데이터

* *숫자형 데이터는 정수, 실수, 복소수, 2진수, 8진수, 16진수를 포함하며 가장 많이 쓰이는 datatype 중 하나입니다.*
* *우리가 알고있는 연산 대부분을 그대로 지원합니다.*

**정수형(integer)**

    a = 10
    b = 5
    
    a + b 
    
**실수형(floating point)**

    c = 3.14
    d = 4
    
    c - d
    
**사칙연산**

```python
a = 10
b = 3

print(a*b)
```

```python
a = 10.3
b = -1.6

print(a*b)
```

**특수연산**

```python
a = 5
b = 2
print(a ** b) #a를 b번 곱한 것 a^b
print(a // b) #a를 b로 나눈 몫
print(a % b0) #a를 b로 나눈 나머지
```
## 2.2 문자열(string)

* *문자열 데이터 타입이란 문자(chracter)의 나열을 의미합니다.
* *파이썬에서는 ' 와 " 를 이용해 문자열을 나타냅니다. ex)'Hello' , "World"

### 2.2.1 문자열을 만드는 방법

    s = 'Hello World'
    s2 = "Goodbye World"
    
### 2.2.2 특수문자표현(escape code)

    \n    newline
    \t    tab
    
### 2.2.3 문자열 연산

```python
s = "Hello"
s2 = "World"

print(s + s2)
```

```python
s = "hi"
print(s * 8)
```
```python
s = "The world is all one"
print(len(s))    #len은 길이를 구하는 함수입니다.
```

### 2.2.4 문자열 formating

문자열을 출력할 때 특정 format을 정해주어 출력할 수 있습니다.

문자열 formating에는 총 3가지 방법이 있습니다. 각 방법은 동일한 결과를 나타내나 조금씩 그 용도가 다르니 알아두시길 바랍니다.

%s는 문자열(string)데이터 타입을 나타내며 %d는 정수형(integer) %f는 실수형(floating point)를 나타냅니다.

* *print format 사용*
```python
apple = "사과"
count = 4
print("%s는 %d개 있다" %(apple, count))
```
* *str.format함수 사용*
```python
print({}는 {}개 있다".format(apple, count))
```

* *f-string*
```python
print(f{apple}는 {count}개 있다.")
```

### 2.2.5 문자열 관련 함수들

* *대소문자 바꾸가upper(),lower()*

```python
s = 'hi'
print(s.upper())

s2 = 'HI'
print(s.lower())
```

* *공백지우기 strip()*

```python
s = "    Life is beautifull" #strip함수는 왼쪽과 오른쪽에서 ()안의 문자열을 제거합니다. 비워놓을시 공백을 제거하며 ()이외의 문자가 나올 때 까지만 지속합니다
print(s.strip())
```

* *문자열 삽입 join()*

```python
s = "HELLO"
print(" ".join(s)) #띄어쓰기 공백을 각 문자열 사이에 추가해 줍니다
```

* *문자열 나누기 split()*

```python
s = "The pig cannnot fly is just a pig"
print(s.split())
```

* *문자열 바꾸기 replace()*

```python
s = "Life is too short"
s.replace("Life", "pencil")
```

## 2.3 연속형 데이터 (Sequential Data Types)

### 2.3.1 리스트 (List)
 * *가장 많이 사용하는 연속형 데이터 타입입니다.*
 * *쉼표로 구분하며 수정이 자유롭습니다*
 * *리스트는 [ 과 ] 를 사용하여 생성합니다.*
 * *리스트 안의 원소는 다양한 데이터 타입을 사용할 수 있습니다. 심지어 리스트도 가능합니다.*
 
 ```python
 L = [1, 2, 3]
 print(type(L))
 L = [1, 2, "Hello", [1, 2], 3.14]
 ```
 
 * *인덱스(index)를 사용하여 원소를 호출(call)할 수 있습니다.*
 * *인덱스는 0부터 시작하며 0, 1, 2, 3, ... 순으로 순차적으로 배정됩니다.*
 * *[ ]를 통해 사용할 수 있으며 -1 인덱스는 마지막에서 바로 전 인덱스를 의미합니다.*
 
 ```python
 L = [1, 2, 3, 4, 5, 6]
 print(L[0])
 print(L[1])
 print(L[2])
 print(L[-1])
 print(L[:])    #L의 원소 전체
 print(L[:3])    #L의 첫 원소부터 4번째 원소까지
 print(L[2:])    #L의 세번째 원소부터 끝까지
 ```

### 2.3.2 리스트 연산
 
 
 ```python
 L = [1, 2, 3]
 L2 = [4, 5]
 print(L + L2)
 
 print(L * 2)
 
 #리스트 수정
 L[0] = 0
 ```
 
 
### 2.3.3 리스트 관련 함수
 
 
 * *리스트에 원소 추가하기 append()*
 
 ```python
 L = [1, 2, 3, 4, 5]
 print(L.append(6))
 ```
 
 * *리스트원소 정렬하기 sort()*
 
 ```python
 L = [4, 2, 16]
 L.sort()
 print(L)
 ```
 
 * *리스트 원소 지우기 remove()*
 
 ```python
 L = [1, 2, 3, 4, 5]
 L.remove(3)
 print(L)
 ```
 
### 2.3.4 튜플 (Tuple)
 
 튜플은 리스트와 거의 같습니다. 리스트와 다른점은 딱 두가지 있습니다.
 
 * *튜플은 []대신 ()를 사용합니다*
 * *리스트는 생성 후 변경이 가능하나(mutable) 튜플은 생성 후 변경이 불가능합니다.(immutable)*
 
### 2.3.5 집합 (Set)
 
 집합은 수학에서의 집합과 상당히 유사합니다.
 
 * *집합은 {}을 사용하나 후에 나올 사전(dict)또한 {}을 사용하기에 공집합을 만들땐 set()으로 생성해아 합니다.*
 * *집합은 원소의 중복을 허용하지 않습니다. 즉 원소의 종류를 표시하기에 좋습니다.*
 * *집합은 원소의 순서가 존재하지 않기에 indexing을 할 수 없습니다.*
 
 ```python
 
 S = {1, 1, 1, 2, 3, 4, 4, 5}
 print(S)
 ```
 
### 2.3.6 집합의 연산
 
 * *교집합*
 
 ```python
 S1 = {1, 2, 3, 4, 5}
 S2 = {3, 4, 5, 6, 7}
 
 print(S1 & S2)
 ```
 * *합집합*
 
 ```python
 print(S1 | S2)
 ```
 
 * *차집합*
 
 ```python
 print(S1 - S2)
 ```
 
### 2.3.7 사전 (Dictionary)

* *사전은 key값과 value값을 가지는 Data type입니다.*
* *key 값을 사용하기 때문에 숫자 index가 아닌 key를 index로 사용합니다*
* 집합과 마찬가지로 {}을 사용하나 : 를 이용하여 key값 과 value 값을 구분합니다*

```python
D = {"John" : "0012", "Maria" : "1234"}
print(D["John"])
```

### 2.3.8 사전 관련 함수

* *사전의 모든 key값 보기 keys()*

```python
D = {"name" : "Kim", "number" : "01012341234", "birth" : "0823"}
print(D.keys())
```

* *사전의 모든 value값 보기 values()*

```python
print(D.values())
```

* *사전의 key, value값 모두 보기 items()*

```python
print(D.items())
```

* *사전의 원소 가져오기 get()*

```python
print(D.get("name", 0)
```

---

## 마치며

이번 포스팅에선 데이터 타입에 관해서 알아보았습니다.

중간 중간 나오는 예제는 모두 직접 jupyter note로 실행 해보시길 바라며 다음시간엔 지금까지 배운 내용을 바탕으로 예시 문제를 풀어보도록 하겠습니다.



