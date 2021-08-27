---
layout: post
title:  "Coding Test - 2. 이름 추천"
date:   2021-08-09
categories: python CodingTest
---
# 키패드 누르기
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/72410#>
    
---
    
## 코드

```python
def solution(new_id):
    
    # step 1
    new_id.lower()

    # step 2
    allowed_text = ['-', '_', '.']
    recommanded_id = []
    
    for text in new_id:
        if text.isalnum():
            recommanded_id.append(text)

        else:
            if text in allowed_text:
                recommanded_id.append(text)
    
    # step 3
    for index, text in enumerate(recommanded_id):
        try:
            while text == '.' and recommanded_id[index+1] == '.':
                recommanded_id.pop(index)
        except:
            continue

            
    # step 4
    try:
        while recommanded_id[0] == '.':
            recommanded_id.pop(0)
    except:
        1
    try:
        while recommanded_id[-1] == '.':
            recommanded_id.pop(-1)
    except:
        1

    # step 5
    if recommanded_id == []:
        recommanded_id.append('a')
    
    # step 6
    if len(recommanded_id) >= 16:
        recommanded_id = recommanded_id[:15]
    
    # step 4 again
    try:
        while recommanded_id[0] == '.':
            recommanded_id.pop(0)
    except:
        1
    try:
        while recommanded_id[-1] == '.':
            recommanded_id.pop(-1)
    except:
        1
  
        
    # step 7
    if len(recommanded_id) <= 2:
        while len(recommanded_id) < 3:
            recommanded_id.append(recommanded_id[-1])
            
    
    answer = ''.join(recommanded_id)
    
    answer = answer.lower()
    
    return answer
```

---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 1에 해당하는 문제입니다. 