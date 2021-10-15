---
layout: post
title:  "Coding Test - 7. 괄호 변환"
date:   2021-09-08
categories: CodingTest
tags: python CodingTest
---
# 괄호 변환
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/60058#>
    
---
    
## 코드

```python
def solution(p):
    
    def fix(p):
        count = []
        for index, m in enumerate(p):
            if m == '(':
                count.append(1)
            elif m == ')':
                count.append(-1)

            if sum(count) == 0 and count != []:
                if sum(count[:index]) < 0:
                    break
                else:
                    return p
 
        if p == '':
            return p
        else:
            p = p[1:-1]
            temp_p = ''
            for a in p:
                if a == '(':
                    temp_p += ')'
                else:
                    temp_p += '('
            p = temp_p
            return p
    
    # --------------------------------------------------------
    def checking(p, answer = ''):
        count = []
        for index, m in enumerate(p):
            if m == '(':
                count.append(1)
            elif m == ')':
                count.append(-1)

            if sum(count) == 0:
                if sum(count[:index]) < 0:
                    answer += '(' + checking(p[index + 1:]) + ')'
                    answer += fix(p[:index + 1])
                    return answer
                else:
                    answer += p[:index + 1]
                    return checking(p[index + 1:], answer)

        if count == []:
            return answer

# -------------------------------------------------------------


    answer = checking(p)
    return answer
```
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 2에 해당하는 문제입니다. 