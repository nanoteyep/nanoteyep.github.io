---
layout: post
title:  "Programmers - 오픈채팅방"
date:   2021-09-07
categories: CodingTest
tags: python CodingTest
---
# 오픈채팅방
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/42888>
    
---
    
## 코드

```python
def solution(record):
    
    u_info = {}
    answer = []
    for query in record:
        order = query.split()[0]
        uid = query.split()[1]
        
        
        if order == 'Enter':
            u_info[uid] = query.split()[2]
        elif order == 'Change':
            u_info[uid] = query.split()[2]
            
    for query in record:
        order = query.split()[0]
        uid = query.split()[1]
        
        if order == 'Enter':
            answer.append(f'{u_info[uid]}님이 들어왔습니다.')
        elif order == "Leave":
            answer.append(f'{u_info[uid]}님이 나갔습니다.')
        
    

    
    return answer
```
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 2에 해당하는 문제입니다. 