---
layout: post
title:  "Road to DataScientist - 4.Python 기초문법_2_Control statement"
date:   2021-07-21 
categories: DataScience python 
---
# Python 기초문법_2
---

## Control Statement란

* *프로그램의 흐름을 제어하는 명령어 입니다.*
* *조건과 반복에 따라 프로그램의 진행 과정이 바뀝니다.*
* *조건(conditional statement)로는 if문이 있습니다.*
* *반복(Iterative statement)로는 while, for 문이 있습니다.*

## 1. If statement(조건문)

* *프로그램에서 가장 중요한 조건 판단 입니다.*
* *파이썬은 if, elif, else구문을 사용하여 조건을 사용할 수 있습니다.* 
* *if문은 : 을 이용해 시작하며 들여쓰기를 통해 각 구문을 구분합니다.*

* 같다 *==* 다르다 *!=* 크다 *>* 작다 *<* 크거나 같다 *>=* 작거나 같다 *<=* 와 같은 비교연산을 사용할 줄 알아야 합니다.

``` 
간단한 예시를 만들어 보겠습니다. 커피를 판매하는 자판기를 구현해 볼 생각입니다. 커피는 300원이며 300원을 넣을 시 커피를 주며 300원 보다 적을시 돈을 돌려주고 300보다 많다면 커피와 거스름돈을 돌려줍니다.
```
```python
money = 300

if money == 300:
    print("coffee")
```
```python
money = 200

if money == 300:
    print("coffee")
else:
    print("돈을 더 넣어주세요")

money = 500

if money == 300:
    print("coffee")
elif money > 300:
    print("coffee")
    print("%d원의 거스름돈 입니다." %(money-300))
else:
    print("돈을 더 넣어주세요")
```

## 2. While 반복문

* while문은 조건을 만족할 때 까지 반복합니다.
```
while(조건):
    statement1
    statement2
```        
* 조건이 true인 동안 statement 1, 2를 반복해서 실행합니다. 

```python
# 2단을 while문으로 구현해봅시다.
number = 1
while(number <= 9):
    print("2 X %d = %d" %(number, (number)*2))
    number = number + 1
```

실제 자판기는 커피가 다떨어지면 작동을 멈춥니다.

while문을 사용하여 구현해 보겠습니다.

```python
# 자판기의 커피 수량
coffee = 5

# 커피가 남아있는 동안 작동!
while coffee > 0:

    # 실제로는 자판기를 통해서 넣은 금액.
    money = int(input("금액을 입력해주세요 : "))
    
    if money == 300:
        # 실제로 이 파트는 자판기에서 커피를 뽑는 명령으로 대체된다.
        print("Coffee")
        # 이제 커피를 하나씩 줄인다.
        coffee = coffee - 1

    elif money < 300:
        # 실제로 이 파트는 돈을 반환한다.
        print("%d원을 돌려줍니다." % money)
        
     
    elif money > 300:

        # 커피를 뽑아주고
        print("Coffee")
        # 이제 커피를 하나씩 줄인다.
        coffee = coffee -1
        # 거스름돈을 돌려준다.
        print("거스름돈은 %d원 입니다."%(money-300))
        
    # 커피가 다 떨어진 경우 알려야한다.
print("커피가 모두 소진되었으니, 관리자에게 문의해주세요.")
```

## 3. for 반복문

* for문은 while문과 다르게 지정된 횟수만큼 반복합니다.
```
for 변수 in 리스트(튜플, 리스트, 문자열):
    statement1
    statement2
```
* 리스트 안의 모든 원소들을 끝까지 반복합니다.

```python
# 원소가 1, 2, 3인 리스트의 원소를 하나하나 출력하는 반복문을 만든다.
L = [1, 2, 3]
for x in L:
    print(x)
```

* range함수를 사용하여 특정 횟수를 자동으로 만들어 줄 수 있습니다.
ex) range(1,5)는 1,2,3,4를 차례대로 생성해 줍니다. (1<= <5)

```python
for x in range(1,5):
    print(x)
```

## 4. 반복문을 제어하는 break, continue

* 반복문을 탈출하는 명령어 입니다.
> break

* 특정 조건에만 반복문을 건너 뜁니다.
> continue

```python
#### 자판기의 커피 수량
coffee = 5

# 일단 작동!
while true:

    # 실제로는 자판기를 통해서 넣은 금액.
    money = int(input("금액을 입력해주세요 : "))
    # 

    
    if money == 300:
        # 실제로 이 파트는 자판기에서 커피를 뽑는 명령으로 대체된다.
        print("Coffee")
        # 이제 커피를 하나씩 줄인다.
        coffee = coffee - 1

    elif money < 300:
        # 돈을 더 받자.
        print("돈이 모자랍니다. 추가로 금액을 입력해주세요.")
        continue

        
    else: # or elif money > 300:
        print("Coffee")
        # 커피를 뽑아주고
        coffee = coffee - 1
        # 이제 커피를 하나씩 줄인다.
        print("%d원을 반환합니다." % (money-300))
        # 거스름돈을 돌려준다.

    # 정산
    print("커피가 %d잔 남았어요!" %coffee)
    
    if coffe < 0:
    break
        
    # 커피가 다 떨어진 경우 알려야한다.
print("커피가 모두 소진되었으니, 관리자에게 문의해주세요.")
```
* break 와 continue의 역할을 생각해보며 위의 예제를 실행해 봅시다. 비교를 위해 break와 continue를 각각 지우면서 테스트 하면 좋습니다.

---

# 마치며
이번 포스팅에선 조건문과 반복문에 대하여 알아보았습니다.

다음 포스팅에선 이 중요한 문법을 좀 더 자유롭게 쓰기 위하여 예시를 통해 연습해 보고자 합니다.
