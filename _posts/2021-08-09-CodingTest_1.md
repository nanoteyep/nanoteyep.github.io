---
layout: post
title:  "Coding Test - 1. 키패드"
date:   2021-08-09
categories: python CodingTest
---
# 키패드 누르기
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/67256>
    
---
    
## 코드

```python
def solution(numbers, hand):
    # 오른손과 왼손의 첫 위치가 각각 '#', '*' 이므로 이를 숫자 12, 10로 생각하고 할당해줍니다.
    r_position = 12
    l_position = 10
    
    # 변수 초기화 입니다.
    answer = ''
    
 
    for number in numbers:
    
        # 번호 조건에 맞는 손가락을 'temp_answer`에 저장하고 손가락 위치를 조정합니다.
        if number in [1, 4, 7]:
            temp_answer = 'L'
            l_position = number
        elif number in [3, 6, 9]:
            temp_answer = 'R'
            r_position = number
            
        # 만약 들어오는 번호가 0이라면 11로 다시 할당합니다.
        elif number in [2, 5, 8, 0]:
            if number == 0:
                number = 11
            
            # 필요한 이동거리를 계산하기 위한 부분입니다.
            if r_position in [2, 5, 8, 11]:
                r_movement = abs(r_position - number)//3
            else:
                r_movement = abs(r_position - 1 - number)//3 + 1
            
            if l_position in [2, 5, 8, 11]:
                l_movement = abs(l_position - number)//3
            else:
                l_movement = abs(l_position + 1 - number)//3 + 1
            
            # 이동거리가 더 짧은 쪽을 선택하고 또 다시 `temp_answer`에 값을 할당하고 손가락 위치를 조정합니다.
            if r_movement > l_movement:
                temp_answer = 'L'
                l_position = number
            elif r_movement < l_movement:
                temp_answer = 'R'
                r_position = number
            elif r_movement == l_movement:
                # 오른손잡이와 왼손잡이를 판별하는 부분입니다.
                if hand == 'right':
                    temp_answer = 'R'
                    r_position = number
                elif hand == 'left':
                    temp_answer = 'L'
                    l_position = number
        
        # 할당된 'temp_answer'를 answer에 하나씩 이어붙입니다.
        answer += temp_answer
     
    # 첫 초기화때 넣었던 공백을 제거한 후 answer를 출력합니다.
    answer = answer.strip('')
    return answer
```

---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 1에 해당하는 문제입니다. 