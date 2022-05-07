---
layout: post
title:  "Leetcode 2. Add Two Numbers in Python3"
date:   2022-05-07
categories: CodingTest
tags: python CodingTest
---
# Add Two Numbers
---

## 개요

* 이번 포스팅은 파이썬 코딩테스트 연습문제 풀이입니다.
* `Leetcode` 에 공개된 예제이며 링크는 아래와 같습니다.

> <https://leetcode.com/problems/add-two-numbers/>
    
---
    
## 코드

```python
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        
        answer = temp = ListNode(0)
        # 이전 node의 합이 10을 넘겼는지 확인하는 변수
        over_ten = 0

        while l1 or l2:
            added = 0
            if over_ten:
                added += 1
            if l1:
                added += l1.val
                l1 = l1.next
            if l2:
                added += l2.val
                l2 = l2.next
            if added >= 10:
                over_ten = 1
            else:
                over_ten = 0
            temp.next = ListNode(added % 10)
            temp = temp.next
        
        if over_ten:
            temp.next = ListNode(1)
        return answer.next
```

---
# 마치며
이번 포스팅은 `Leetcode`에 존재하는 난이도 Medium 에 해당하는 문제입니다.  