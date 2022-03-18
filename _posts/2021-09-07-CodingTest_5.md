---
layout: post
title:  "Programmers - 문자열 압축"
date:   2021-09-07
categories: CodingTest
tags: python CodingTest
---
# 문자열 압축
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/60057>
    
---
    
## 코드

```python
def solution(s):
    
    temp_answer = []
    
    if len(s) == 1:
        return 1
    
    for length in range(1,len(s)):
        cut_s = []
        for i in range(0, len(s), length):
            cut_s.append(s[i:i+length])

        count = 0
        new_s = ''
        for i in range(1, len(cut_s)): 
            if cut_s[i] == cut_s[i - 1]:
                count += 1
            else:
                if count >= 1:
                    new_s += str(count + 1) + cut_s[i - 1]
                    count = 0
                else:
                    new_s += cut_s[i - 1]
                    count = 0

            if i == len(cut_s) - 1:
                if count >= 1:
                    new_s += str(count + 1) + cut_s[i]
#                     print(new_s)
                    temp_answer.append(len(new_s))
                else:
                    new_s += cut_s[i]
#                     print(new_s)
                    temp_answer.append(len(new_s))

                
#     print(temp_answer)
    answer = min(temp_answer)
    return answer
```
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 2에 해당하는 문제입니다. 