---
layout: post
title:  "Road to DataScientist - 4.Python 기초문법_2_Control statement 예제"
date:   2021-07-21 
categories: DataScience python 
---
# Python 기초문법_2 예제
---

## 1.  if 지옥

* 다음 코드의 실행결과를 예측해보자.

```python
a  = "Life is too short, you need python"

if "wife" in a:
    print("wife")
elif "python" in a and "you" not in a:
    print("python")
elif "shirt" not in a:
    print("shirt")
elif "need" in a:
    print("need")
else:
    print("none")
```
## 2. 약수 찾기

* 100 이하의 자연수 중에서 5의 배수를 모두 찾아서 출력해주세요.

## 3. 별 찍기!

* 하늘의 아름다운 별을 따다 코드로 작성해보자.

```python
#number = 5
*****
****
***
**
*

number = int(input("Input the number of lines : "))
```

## 4. 단어 갯수 구하기

* 다음과 같은 영어기사 5개가 있다.
* 철수는 다음 뉴스기사들의 어떤 단어가 얼마나 나왔는지 궁금해졌다.
* 뉴스기사에서 등장하는 모든 단어마다 개수를 세어보자!

```python
#아래 5개의 변수들은 각각 하나의 문서를 의미합니다.
news1 = "earn	champion products ch approves stock split champion products inc said its board of directors approved a two for one stock split of its common shares for shareholders of record as of april the company also said its board voted to recommend to shareholders at the annual meeting april an increase in the authorized capital stock from five mln to mln shares reuter"
news2 = "acq	computer terminal systems cpml completes sale computer terminal systems inc said it has completed the sale of shares of its common stock and warrants to acquire an additional one mln shares to sedio n v of lugano switzerland for dlrs the company said the warrants are exercisable for five years at a purchase price of dlrs per share computer terminal said sedio also has the right to buy additional shares and increase its total holdings up to pct of the computer terminal s outstanding common stock under certain circumstances involving change of control at the company the company said if the conditions occur the warrants would be exercisable at a price equal to pct of its common stock s market price at the time not to exceed dlrs per share computer terminal also said it sold the technolgy rights to its dot matrix impact technology including any future improvements to woodco inc of houston tex for dlrs but it said it would continue to be the exclusive worldwide licensee of the technology for woodco the company said the moves were part of its reorganization plan and would help pay current operation costs and ensure product delivery computer terminal makes computer generated labels forms tags and ticket printers and terminals reuter"
news3 = "earn	cobanco inc cbco year net shr cts vs dlrs net vs assets mln vs mln deposits mln vs mln loans mln vs mln note th qtr not available year includes extraordinary gain from tax carry forward of dlrs or five cts per shr reuter"
news4 = "earn	am international inc am nd qtr jan oper shr loss two cts vs profit seven cts oper shr profit vs profit revs mln vs mln avg shrs mln vs mln six mths oper shr profit nil vs profit cts oper net profit vs profit revs mln vs mln avg shrs mln vs mln note per shr calculated after payment of preferred dividends results exclude credits of or four cts and or nine cts for qtr and six mths vs or six cts and or cts for prior periods from operating loss carryforwards reuter"
news5 = "earn	brown forman inc bfd th qtr net shr one dlr vs cts net mln vs mln revs mln vs mln nine mths shr dlrs vs dlrs net mln vs mln revs billion vs mln reuter"

#news_list는 뉴스 5개가 원소인 리스트입니다.
#word_Dict는 문서 하나당 단어와 단어 갯수가 저장되는 사전입니다. 하나만 사용하셔도 되고, 여러개를 사용하셔도 됩니다.

```
---
---

## 해답 1.

    shirt

## 해답 2.

```python
for x in range(1,101):
   if (x % 5) == 0:
        print(x)
```

## 해답 3.
```python
number = int(input("Input the number of lines : "))

for x in range(number):
    print('*' * (number-x))
```

## 해답 4.
```python
news_list = news1.split() + news2.split() + news3.split() + news4.split() + news5.split()
word_Dict = {}

for word in news_list:
  word_Dict[word] = word_Dict.get(word, 0) + 1

print(word_Dict)
```

