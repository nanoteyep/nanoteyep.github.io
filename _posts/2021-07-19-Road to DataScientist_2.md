---
layout: post
title:  "Road to DataScientist - 2.환경설정"
date:   2021-07-19 
categories: DataScience python 
---
# 환경설정
---



# 1. Python

제가 데이터 분석을 하기 위해 사용할 툴은 파이썬입니다.

파이썬은 세계에서 가장 많이 활용되고 있는 프로그래밍 언어중 하나이며 다음과 같은 특징을 가지고 있습니다.

* **확장성** - 오픈소스 라이브러리가 많습니다.
* **생산성** - 비교적 간단한 언어로 개발속도가 빠릅니다.
* **가독성** - 코드가 이쁘기 때문에 유지보수가 쉽습니다.

> LIFE IS TOO SHORT, YOU NEED PYTHON
---

# 2. Anaconda

anaconda는 파이썬을 사용함에 있어 가상환경을 구축해주어 버전관리에 굉장히 유용한 툴입니다.

또한 자체적으로 많은 파이썬 패키지들을 포함하고 있기에 일일이 라이브러리를 사용하기 위해 패키지를 다운로드 받아야 하는 번거로움을 줄여줍니다.

## 2.1 anaconda 설치

anaconda는 다음 사이트에서 다운로드 받을 수 있습니다. <https://www.anaconda.com/products/individual#Downloads>

![anaconda_1](/img/anaconda_1.png)

자신에게 맞는 버전을 설치하면 되나 저는 윈도우 64-bit환경에서 작업하기 때문에 윈도우 인스톨러를 다운 받습니다.

설치 옵션 또한 기본옵션으로 그대로 진행하였습니다.

## 2.2 anaconda 설치 확인

anaconda 설치를 완료하셨다면 다음과 같이 시작메뉴에서 확인 하실 수 있습니다.

![anaconda_2](/img/anaconda_2.png)

이 중 Anaconda Powershell Prompt를 실행하여 다음과 같이 버전확인을 완료하셨으면 설치가 정상적으로 완료된 것 입니다.

![anaconda_3](/img/anaconda_3.png)

## 2.3 가상환경 구축

anaconda는 가상환경을 구축하여 작업을 함으로서 버전 충돌 등을 막아주는 기능을 합니다.

가상환경 구축을 위해 버전확인을 했던 명령프롬프트에 다음과 같이 입력해 줍니다.

    conda create -n datascience python=3.8
    
datascience 라는 가상환경을 python 3.8 버전으로 구축하겠다는 명령어 입니다. 파이썬 버전 이후 anaconda라는 명령어를 추가하시면 anaconda상에 설치되어 있는 모든 패키지를 동시에 다운받을 수 있지만 저는 그때그때 필요한 라이브러리를 다운받으면서 하기 위해 생략해 주었습니다.

생략하면 miniconda라는 필요한 라이브러리만 다운받게 됩니다. 이후 설치하고자 하는 목록이 나타나시면 y키를 눌러 설치를 진행해주시면 됩니다.

    conda activate datascience
    
를 통해 datascience라는 가상환경에 들어갈 수 있으며

    conda deactivate

를 통해 현재 진입되어 있는 가상환경에서 빠져나올 수 있습니다.

---

# 3. Jupyter Notebook

jupyter notebook은 web 환경에서 python을 interactive하게 다룰 수 있는 툴입니다.

특별한 설치과정 없이 anaconda를 설치 할 때 같이 설치되며

설치된 jupyter notebook 실행파일을 직접 실행하거나 anaconda prompt에서 jupyter notebook을 입력해 실행할 수 도 있습니다.


## 3.1 사용법

jupyter notebook에서 python 파일을 만들면 글과 그림을 작성하는 markdown cell 과 코드를 작성하는 code cell로 구분되어 있습니다.

두 cell은 단축키 m버튼을 통하여 변환하며 자유롭게 편집할 수 있습니다.

code mode에서 python 코드를 입력 후 Ctrl+Enter 버튼을 통해 실행 결과를 cell단위로 확인 할 수 있습니다.

## 3.2 예시

jupyter notebook으로 python파일을 만들고 직접 실험해 봅시다.

code cell에 다음과 같이 입력해 봅시다.

```python
print("hello world")
```

Ctrl+Enter 버튼을 눌러 실행해 보신다면

        In [1]: print("hello world")
        
        hello world
        
이런식으로 작동된 python 코드를 확인 할 수 있을 것 입니다.

---

# 마치며

이번에는 데이터 분석을 위한 환경설정 방법에 대해 알아보았습니다.

다음에는 이렇게 구축된 환경을 이용하여 직접 파이썬 예제를 통해 연습을 해보려고 합니다.