---
layout: post
title:  "Coding Test - 3. 메뉴 리뉴얼"
date:   2021-09-02
categories: python CodingTest
---
# 신규 이름 추천
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/72411>
    
---
    
## 코드

```python
def solution(orders, course):
    
    def sorting(data):
        temp = []
        for i in data:
            temp += i

        temp.sort()
        value = ''
        
        for m in temp:
            value += m

        return value

    
    def combination(order, number):
        
        order_set = []
        
        if number <= 1:
            return order
        
        for m in range(len(order)):
            for n in combination(order[m+1:], number -1):
                order_set.append(sorting(order[m]+n))
        
        return order_set
        
# ---------------------------------------------------
    
    answer = []
    for number in course:
        set = []
        for order in orders:
            set += combination(order, number)
        
        count = 2
        selected_course = []
        for i in set:
            if count <= set.count(i):
                if count == set.count(i):
                    selected_course += [i]
                    continue
                count = set.count(i)
                selected_course = [i]
            
        
        
        answer += selected_course
        
    temp = ''
    temp_answer = []
    answer.sort()
    for i in answer:
        if temp == i:
            continue
        temp = i
        temp_answer.append(i)


    answer = temp_answer
    
    
    
    return answer
```

---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 2에 해당하는 문제입니다. 