---
layout: post
title:  "Coding Test - 8. 완주하지 못한 선수"
date:   2022-01-10
categories: CodingTest
tags: python CodingTest
---
# 완주하지 못한 선수
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/42576>
    
---
    
## 코드

```python
def solution(participant, completion):
    d = dict()
    
    for p in participant:
        d[p] = d.get(p, 0) + 1
    for c in completion:
        d[c] += -1
        if d[c] == 0:
            del d[c]
            
    
    answer = list(d.keys())[0]
    return answer
```
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 해시 예제 중 레벨 1에 해당하는 문제입니다. 