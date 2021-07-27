---
layout: post
title:  "Kaggle - 1.Titanic"
date:   2021-07-27 
categories: DataScience python kaggle titanic
---
# kaggle competition - 1. Titanic
---

## 시작하기에 앞서

이번 포스팅은 각종 데이터 분석 대회를 주최하는 `kaggle`의 듀토리얼격인 `titanic`에 직접 참가해 보며 알아보겠습니다.

저도 `kaggle`은 처음 이라 `Subin An`님의 code를 사용하였습니다. 감사합니다.

> <https://www.kaggle.com/subinium/subinium-tutorial-titanic-beginner>

---

## 1. kaggle알아보기

* kaggle competition에 참가하기 위해선 회원가입이 필요합니다.
* 가입을 완료했다면 `competition`란에서 `titanic`을 찾아갑니다.

![titanic_1](/img/titanic_1.PNG)

* 전 이미 `competition`에 참가중이기 때문에 `Submit Predictions`으로 활성화 되있습니다만 처음이시라면 `Join Competition`이 활성화 되어 있을 겁니다.
* `Join Competition`을 클릭하여 `titanic`에 참가합니다.
![titanic_2](/img/titanic_2.png)

* `code`란에 가시면 사진과 같이 `New Notebook`란이 활성화 됩니다.
* `Notebook`은 `kaggle`에서 제공하는 데이터 분석용 프로그래밍 환경입니다.
* 기본적으로 여러가지 라이브러리가 설치되어 있으며 데이터 분석에 필요한 리소스또한 제공합니다.
* 또한 `Competition`에 참가중이라면 다음과 같이 자동으로 `data`를 추가해 줍니다.

![titanic_3](/img/titanic_3.png)

* 클릭하면 경로를 복사할 수 있으며 이 방법 이외에도 `data`란에서 직접 데이터를 다운로드 받을 수도 있습니다.

---

## 2. 라이브러리 불러오기

```python
# 필요한 라이브러리를 우선 불러옵니다.

## 데이터 분석 관련
import pandas as pd
from pandas import Series, DataFrame
import numpy as np

## 데이터 시각화 관련
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid') # matplotlib의 스타일에 관련한 함
## 그래프 출력에 필요한 IPython 명령어
%matplotlib inline 

## Scikit-Learn의 다양한 머신러닝 모듈을 불러옵니다.
## 분류 알고리즘 중에서 선형회귀, 서포트벡터머신, 랜덤포레스트, K-최근접이웃 알고리즘을 사용해보려고 합니다.
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC, LinearSVC
from sklearn.ensemble import RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
```

* 라이브러리는 크게 3가지 분류로 나눌 수 있습니다.
* `데이터 분석`, `시각화`, `머신러닝`
* 머신러닝에 처음보는 라이브러리가 다수 있지만 머신러닝에 관련된 포스팅은 추후에 따로 포스팅 하겠습니다.

---

## 3. 데이터 읽어들이고 살펴보기


* 코드를 살펴보기 앞서 `titanic`의 데이터를 먼저 살펴보도록 하겠습니다.
* `titanic`은 `gender_submission.csv`, `train.csv`, `test.csv` 세가지 데이터 파일이 존재합니다.
* `gender_submission.csv`는 우리가 `competition`에 제출해야하는 형식의 예시파일 입니다.
* `titanic`에서 `gender_submission.csv`는 `PassengerId`와 `Survived`를 표시해서 제출해야 함을 알려주고 있습니다.
* `train.csv`는 `tatinic`이라는 실질적인 데이터입니다.
* 승객의 정보, 이 승객이 살아남았는지를 기록한 실제 데이터입니다.
* `test.csv`는 우리가 `train.csv`를 통해 학습한 모델을 적용시켜야 하는 문제 파일입니다.
* 학습된 모델로 `test.csv`의 정보로 기록된 승객이 살아 남을 것인가 예측 하는 것이 `titanic`의 최종목표입니다.

```python
# 데이터를 우선 가져와야합니다.
train_df = pd.read_csv("../input/titanic/train.csv")
test_df = pd.read_csv("../input/titanic/test.csv")

# 데이터 미리보기
train_df.tail()
```

* 데이터를 불러들이고 제대로 불러 들어왔는지 확인하는 작업입니다.
* 데이터의 경로는 오른쪽 상단에서 경로를 복사하고 확인 할 수 있습니다.

```python
train_df.info()
print('-'*60)
test_df.info()
```

* info메소드를 이용하여 데이터의 정보를 확인합니다.

![titanic_4](/img/titanic_4.png)

* 각각 구성된 `columns`값, `몇 개의 데이터`, `데이터 타입`을 확인 할 수 있습니다.
* `train_df`부터 살펴보겠습니다.
* 총 891개의 승객 데이터가 있으며 `Age`, `Cabin`, `Embarked`에는 `null`값이 채워져 있는 칸이 있다는 것을 알 수 있습니다.
* `test_df`또한 살펴 보겠습니다.
* `Survived` column이 없으며 418개의 데이터가 있습니다.
* 또한 `Age`, `Fare`, `Cabin`의 몇몇 값에는 `null`값이 채워져 있는 것을 알 수 있습니다.

---

## 4. 데이터 가공하기

* 가장 먼저 생존률과 관계 없어보이는, 이번 모델에서 사용하지 않을 column을 선별해 지워줍니다.
* `승객ID`와 `이름` 그리고 `티켓`값은 생존률과 아무 상관이 없다고 생각하여 과감하게 지워주었습니다.
* `test.df`에서 `PassengerId`를 지우지 않은 이유는 저희가 제출해야하는 데이터에 포함되어 있기 때문입니다.

```python
train_df = train_df.drop(['PassengerId', 'Name', 'Ticket'], axis=1)
test_df = test_df.drop(['Name','Ticket'], axis=1)
```
![titanic_5](/img/titanic_5.png)

* 이제 이러한 데이터를 가지고 하나씩 가공해 보도록 하겠습니다.

### 4.1 Pclass

* `train_df`의 `Survived`를 제외한 첫번째 `column`인 `Pclass`를 가공해 보도록 하겠습니다.

```python
train_df["Pclass"].value_counts()
```
![titanic_6](/img/titanic_6.png)

* `Pclass`는 1, 2, 3이라는 세가지 값을 가지고 있습니다.
* 1등급, 2등급, 3등급이라는 의미의 `Pclass`는 수치적 의미보단 범주형(카테고리형) 데이터 이므로 `one-hot encoding`을 사용하겠습니다.
* `one-hot encoding`은 `pd.get_dummies()`라는 메소드를 이용하려고 합니다.
* `pd.get_dummies()`메소드는 범주형 데이터를 0과 1로 구성된 `DataFrame`을 생성해 주는 메소드 이기 때문에 자주 쓰이는 `one-hot encoding`방법 입니다.

```python
# get_dummies메소드를 이용하여 "Pclass" column만 one-hot encoding합니다.
Pclass_train_dummies = pd.get_dummies(train_df["Pclass"])
Pclass_test_dummies = pd.get_dummies(test_df["Pclass"])

Pclass_train_dummies.columns = ['1st', '2nd', '3rd']
Pclass_test_dummies.columns = ['1st', '2nd', '3rd']

train_df.drop(["Pclass"], axis=1, inplace=True)
test_df.drop(["Pclass"], axis=1, inplace=True)

train_df = train_df.join(Pclass_train_dummies)
test_df = test_df.join(Pclass_test_dummies)
```