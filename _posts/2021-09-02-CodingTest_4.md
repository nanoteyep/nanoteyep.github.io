---
layout: post
title:  "Programmers - 순위 검색"
date:   2021-09-02
categories: CodingTest
tags: python CodingTest
---
# 순위 검색
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Programmers` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://programmers.co.kr/learn/courses/30/lessons/72412>
    
---
    
## 코드

```python
def solution(info, query):
    
    class Member_Info:
        def __init__(self, data):
            self.lang = data[0]
            self.job_group = data[1]
            self.career = data[2]
            self.soul_food = data[3]
            self.score = data[4]
            self.down = None
            
    class Member_Search:
        def __init__(self, data):
            self.head = Member_Info(data)
            
        def insert(self, data):
            self.current_node = self.head
            while True:
                if self.current_node.down == None:
                    self.current_node.down = Member_Info(data)
                    break
                else:
                    self.current_node = self.current_node.down
                    
        def search(self, target):
            self.current_node = self.head
            
            count = 0
            while self.current_node:
                if target[0] == '-' or self.current_node.lang == target[0]:
                    if target[1] == '-' or self.current_node.job_group == target[1]:
                        if target[2] == '-' or self.current_node.career == target[2]:
                            if target[3] == '-' or self.current_node.soul_food == target[3]:
                                if int(self.current_node.score) >= int(target[4]):
                                    count += 1
                                    
                self.current_node = self.current_node.down
                
            return count
                
            
#     -------------------------

    head = info[0]
    head = head.split()
    tree = Member_Search(head)
    
    for data in info[1:]:
        data = data.split()
        tree.insert(data)
        
    answer = []
    
    for target_list in query:
        target_list = target_list.split()
        target = []
        target.append(target_list[0])
        target.append(target_list[2])
        target.append(target_list[4])
        target.append(target_list[6])
        target.append(target_list[7])

        
        count = tree.search(target)
        
        answer.append(count)
        
    return answer
```
* 위 코드는 답은 다 맞았으나 효율성에서 점수를 전혀 받지 못하였습니다.
---
# 마치며
이번 포스팅은 `Programmers`에 존재하는 파이썬 코딩테스트 예제 중 레벨 2에 해당하는 문제입니다. 