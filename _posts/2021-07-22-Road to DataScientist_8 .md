---
layout: post
title:  "Road to DataScientist - 5.Python 기초문법_4_I/O"
date:   2021-07-22 
categories: DataScience python 
---
# Python 기초문법_4
---

## 1. I/O란?

* 프로그램 입장에서 들어오는 데이터를 *input* 나가는 데이터를 *output*이라고 부릅니다.
> 두 개념을 합쳐 *I/O*라고 부릅니다.
* 사용자의 키보드로 부터 입력 받는 것을 *stdin*, 모니터로 출력되는 것을 *stdout*이라고 부릅니다.
* 메인 메모리상에서 돌아가는 프로그램 이외에, 스토리지로 부터 file을 불러오는 것도 *input*, 메모리로부터 스토리지로 file을 저장하는 것 또한 *outout*입니다.
> 스토리지와 프로그램 사이의 I/O를 *file I/O*라고 합니다.

## 2. Stdin/Stdout in python

* 파이썬은 *input()* 을 통해 사용자로부터 *stdin*을 받아들일 수 있습니다.
* 파이썬은 *print()* 를 통해 사용자에게 *stdout*을 출력할 수 있습니다.

```python
a = input("출력할 값을 입력해 주세요 : ")
print(a)
```

* *input()*으로 입력된 데이터의 형식은 무조건 *str*입니다.

## 3.  File I/O

* python에서는 *open* 함수를 통해서 쉽게 파일을 열 수 있습니다.
* *open* 함수를 통해 연 파일은 *close* 함수를 통해 닫아주어야 합니다.
* 이와 같은 번거로움을 피하기 위해 *with open*을 사용하게 된다면 자동으로 구문이 끝난 후 파일을 닫아 줍니다.
* *open()* 함수는 다양한 옵션을 제공하지만 기본적으론 *txt*, 다른 파일 형식을 열기 위해선 다른 라이브러리가 필요합니다.

```python
with open("data/test.txt", "r", encoding = "utf-8") as f:   #코드의 현재경로에 있는 폴더 중 data라는 폴더에 입장하여 test.txt라는 파일을 엽니다.
    data = f.read()    #read는 파일 안의 모든 글자를 string으로 저장합니다.
    data = f.readline()    #readline은 파일 안의 첫 줄의 글자를 string으로 저장합니다.
    data = f.readlines()    #readlines는 파일 안의 모든 글자를 줄단위로 list에 저장합니다.
    
    for line in f:
        print(line)    #for 문을 이용해도 파일 안의 글자를 읽어들일 수 있습니다.
        
```

> 파일의 경로에 주의합시다. ./는 현재 경로를 의미하며 ../는 현재 경로의 한단계 상위 경로를 의미합니다. 위 예시에서의 경로는 현재 경로에서 data라는 폴더를 탐색하여 입장 한 후 test.txt를 실행한다는 의미입니다.

---
# 마치며

I/O에 대하여 간략하게 알아보았습니다. file I/O같은 경우 더 다양한 응용이 가능하며 다양한 라이브러리가 있으니 직접 파일을 만들어 여러가지 상황을 테스트 해보길 바랍니다. 지금까지 파이썬의 기초 문법을 간략하게 알아보았습니다. 파이썬을 사용 할 줄 아는 것은 대단히 중요하니 모자란 부분은 직접 연습해 보시길 바랍니다. 다음 포스팅 부터는 파이썬 기초문법을 마치고 데이터 분석을 하기 위한 numpy에 대해 알아보고자 합니다.