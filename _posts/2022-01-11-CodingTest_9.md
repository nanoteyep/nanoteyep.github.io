---
layout: post
title:  "Coding Test - 전화번호 목록"
date:   2022-01-11
categories: CodingTest
tags: python CodingTest
---
# 전화번호 목록
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/42577>
    
---
    
## 코드

```python
def solution(phone_book):
    answer = True
    
    d = [{} for _ in range(20)]
    for num in phone_book:
        d[len(num)-1][num] = True
    for num in phone_book:
        for sub_idx in range(len(num)-1):
            try:
                if d[sub_idx][num[:(sub_idx+1)]]:
                    answer = False
                    return answer
            except:
                pass
    return answer
```
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 해시 예제 중 레벨 2에 해당하는 문제입니다. 