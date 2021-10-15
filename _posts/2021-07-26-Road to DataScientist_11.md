---
layout: post
title:  "Road to DataScientist - 8.실습"
date:   2021-07-26 
categories: DataScience 
tags: DataScience python
---
# 실습
---

## 공공 데이터를 이용한 카페 상권 분석

* 공공 데이터 포털(data.go.kr)에는 다양한 데이터가 공개되어 있습니다.
* 그 중 이번 포스팅에선 다양한 상권 업종 중 카페에 관해 현황을 조사하려고 합니다.

***목표***

1. 전국 카페 데이터를 수집합니다.
2. 지역별, 브랜드별 데이터를 수집합니다.
3. 분석결과를 시각화 합니다.

***데이터***

https://www.data.go.kr/data/15083033/fileData.do


## 1. 라이브러리 불러오기

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

## 2. 데이터 불러오기

```python
data = pd.read_csv("../data/소상공인시장진흥공단_상가(상권)정보_20210331/소상공인시장진흥공단_상가(상권)정보_강원_202103.csv")  # 저는 코드 바로 상위폴더에 다운로드 받은 데이터를 저장하였습니다. 각자 자신의 경로에서 읽어들이면 됩니다.
data
``` 
* data변수를 출력해 보아 어떤 형식으로 데이터가 생겼는지 확인해 봅니다.
* 이제는 다운로드 받은 모든 csv파일을 읽어들여 데이터로 가공해 보겠습니다.

```python
from glob import glob   # 폴더 안의 모든 파일을 읽어들이기 위한 라이브러리 입니다.

# csv 목록 불러오기
file_names = glob("../data/소상공인시장진흥공단_상가(상권)정보_20210331/*.csv")
total = pd.DataFrame()

# 모든 csv 병합하기
for file_name in file_names:
    data = pd.read_csv(file_name, encoding='utf-8')
    total = pd.concat([total, data])   #concat은 DataFrame에서의 append와 비슷한 기능입니다.

# reset index
total.reset_index(drop = True, inplace = True)  # 여러개의 DataFrame을 이어 붙였기 때문에 인덱스가 뒤죽박죽입니다. reset_index는 이러한 인덱스를 초기화 해주며 drop은 기존 index를 삭제할지, inplace는 이렇게 수정된 index를 원본에 덮어 씌울지를 의미합니다.
total
```
```
# 분석에 필요한 column을 고릅니다. ## 자유롭게 하셔도 상관없습니다.
data = total[[ '상호명', '상권업종대분류명', '상권업종중분류명', '도로명주소', '시도명', '시군구명']]
data

# 메모리 낭비를 막기 위해 필요없는 변수는 제거합니다.
del total
```

## 3. 데이터 가공

* 우선 전국의 커피 전문점 정보를 가공합니다.

```python
# 카페만 뽑아냅니다.
#set(data["상권업종중분류명"])    #key값 확인
df_coffee = data[data['상권업종중분류명'] == '커피점/카페']
# index를 다시 세팅합니다.
df_coffee.index = range(len(df_coffee))
print("전국 커피 전문점 점포 수 : ", len(df_coffee))
df_coffee.head()
```

* 다음은 서울의 커피 전문점 정보를 가공합니다.

```python
# 카페 중에 "서울"에 위치하고 있는 점포만 뽑아냅니다.
# set(data["시도명"])  # key값 확인
df_seoul_coffee = data[data["시도명"] == "서울특별시"]
df_seoul_coffee.index = range(len(df_seoul_coffee))
print('서울시 내 커피 전문점 점포 수 :', len(df_seoul_coffee))
df_seoul_coffee.head()
```
* 지금부터 전국의, 서울시 내의 스타벅스, 이디야, 커피빈, 투썸플레이스, 빽다방, 할리스, 메가커피로 데이터를 가공해 보려고 합니다.

```python
# 이번엔 전국에 있는 스타벅스를 뽑아냅니다.
df_starbuks = df_coffee[df_coffee["상호명"].str.contains("스타벅스")]
df_starbucks.index = range(len(df_starbucks))
print('전국 스타벅스 점포 수 :', len(df_starbucks))
df_starbucks.head()

# 이번엔 서울에 있는 스타벅스를 뽑아냅니다.
df_seoul_starbucks = df_coffee[(df_coffee["상호명"].str.contains("스타벅스")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_starbucks.index = range(len(df_seoul_starbucks))
print('서울시 내 스타벅스 점포 수 :', len(df_seoul_starbucks))
df_seoul_starbucks.head()

# 전국 이디야
df_ediya = df_coffee[df_coffee["상호명"].str.contains("이디야")]
df_ediya.index = range(len(df_ediya))
print("점포수 : ", len(df_ediya))
df_ediya.head()

# 서울 이디야
df_seoul_ediya = df_coffee[(df_coffee["상호명"].str.contains("이디야")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_ediya.index = range(len(df_seoul_ediya))
print("점포수 : ", len(df_seoul_ediya))
df_seoul_ediya

#전국 커피빈
df_coffeebin = df_coffee[df_coffee["상호명"].str.contains("커피빈")]
df_coffeebin.index = range(len(df_coffeebin))
print("점포수 : ", len(df_coffeebin))
df_coffeebin

#서울 커피빈
df_seoul_coffeebin = df_coffee[(df_coffee["상호명"].str.contains("커피빈")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_coffeebin.index = range(len(df_seoul_coffeebin))
print("점포수 : ", len(df_seoul_coffeebin))
df_seoul_coffeebin

#전국 투썸
df_2some = df_coffee[df_coffee["상호명"].str.contains("투썸")]
df_2some.index = range(len(df_2some))
print("점포수 : ", len(df_2some))
df_2some

#서울 투썸
df_seoul_2some = df_coffee[(df_coffee["상호명"].str.contains("투썸")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_2some.index = range(len(df_seoul_2some))
print("점포수 : ", len(df_seoul_2some))
df_seoul_2some

#전국 빽다방
df_bbaek = df_coffee[df_coffee["상호명"].str.contains("빽다방")]
df_bbaek.index = range(len(df_bbaek))
print("점포수 : ", len(df_bbaek))
df_bbaek

#서울 빽다방
df_seoul_bbaek = df_coffee[(df_coffee["상호명"].str.contains("빽다방")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_bbaek.index = range(len(df_seoul_bbaek))
print("점포수 : ", len(df_seoul_bbaek))
df_seoul_bbaek

#전국 할리스
df_hollys = df_coffee[df_coffee["상호명"].str.contains("할리스")]
df_hollys.index = range(len(df_hollys))
print("점포수 : ", len(df_hollys))
df_hollys

#서울 할리스
df_seoul_hollys = df_coffee[(df_coffee["상호명"].str.contains("할리스")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_hollys.index = range(len(df_seoul_hollys))
print("점포수 : ", len(df_seoul_hollys))
df_seoul_hollys

# 전국 메가커피
df_mega = df_coffee[df_coffee["상호명"].str.contains("메가커피")]
df_mega.index = range(len(df_mega))
print("점포수 : ", len(df_mega))
df_mega

# 서울 메가커피
df_seoul_mega = df_coffee[(df_coffee["상호명"].str.contains("메가커피")) & (df_coffee["시도명"] == "서울특별시")]
df_seoul_mega.index = range(len(df_seoul_mega))
print("점포수 : ", len(df_seoul_mega))
df_seoul_mega
```
## 4. 데이터 비교

1. 전체 커피 전문점 내 주요 커피 브랜드 입점 비율
```python
print("**** 전국 커피전문점중 주요 5대 커피브랜드 입점 비율 ****")
print("주요 5대 커피브랜드 전국 입점 비율 : %.3f%%" % ((len(df_starbucks)+len(df_2some)+len(df_ediya)+len(df_mega)+len(df_coffeebin))/len(df_coffee)*100))
print("1. 스타벅스 : %.3f%%" % (len(df_starbucks)/len(df_coffee)*100))
print("2. 투썸플레이스 : %.3f%%" % (len(df_2some)/len(df_coffee)*100 ))
print("3. 이디야 : %.3f%%" % (len(df_ediya)/len(df_coffee)*100 ))
print("4. 메가커피 : %.3f%%" % (len(df_mega)/len(df_coffee)*100 ))
print("5. 커피빈 : %.3f%%" % (len(df_coffeebin)/len(df_coffee)*100 ))
```

2. 서울 커피 전문점 내 커피 브랜드 입점 비율
```python
print("스타벅스 : %.3f%%" % (len(df_seoul_starbucks)/len(df_seoul_coffee)*100) )
print("이디야 : %.3f%%" % (len(df_seoul_ediya)/len(df_seoul_coffee)*100) )
print("커피빈 : %.3f%%" % (len(df_seoul_coffeebin)/len(df_seoul_coffee)*100))
print("투썸플레이스 : %.3f%%" % (len(df_seoul_2some)/len(df_seoul_coffee)*100))
print("빽다방 : %.3f%%" % (len(df_seoul_bbaek)/len(df_seoul_coffee)*100))
print("할리스 : %.3f%%" % (len(df_seoul_hollys)/len(df_seoul_coffee)*100))
print("메가커피 : %.3f%%" % (len(df_seoul_mega)/len(df_seoul_coffee)*100))
```

3. 각 브랜드 별 서울 입점 비율
```python
print("**** 주요 5대 커피브랜드별 서울 입점 비율 ****")
print("1. 스타벅스 : %.3f%%" % (len(df_seoul_starbucks)/len(df_starbucks)*100))
print("2. 투썸플레이스 : %.3f%%" %(len(df_seoul_2some)/len(df_2some)*100) )
print("3. 이디야 : %.3f%%" % (len(df_seoul_ediya)/len(df_ediya)*100))
print("4. 메가커피 : %.3f%%" % (len(df_seoul_mega)/len(df_mega)*100))
print("5. 커피빈 : %.3f%%" % (len(df_seoul_coffeebin)/len(df_coffeebin)*100))
```
```python
# 각 구별로 스타벅스가 얼마나 있는지 확인합니다.
starbucks_gu = df_seoul_starbucks.groupby('시군구명')['상호명'].count().to_frame().sort_values(by='상호명', ascending=False)
starbucks_gu = starbucks_gu.reset_index()
starbucks_gu = starbucks_gu.set_index('시군구명')
starbucks_gu
```

## 4. 시각화

```python
# 주요 5대 커피브랜드 서울 입점 비율을 시각화합니다.
starbucks_rate = (len(df_seoul_starbucks) / len(df_seoul_coffee) * 100)
ediya_rate = (len(df_seoul_ediya) / len(df_seoul_coffee)* 100)
hollys_rate = (len(df_seoul_hollys) / len(df_seoul_coffee)* 100)
twosome_rate = (len(df_seoul_2some) / len(df_seoul_coffee)* 100)
mega_rate = (len(df_seoul_mega) / len(df_seoul_coffee)* 100)


# starbucks_rate = (len(df_seoul_starbucks) / len(df_starbucks) * 100)
# twosome_rate = (len(df_seoul_2some) / len(df_2some) * 100)
# ediya_rate = (len(df_seoul_ediya) / len(df_ediya) * 100)
# mega_rate = (len(df_seoul_mega) / len(df_mega) * 100)
# coffeebean_rate = (len(df_seoul_coffeebean) / len(df_coffeebean) * 100)

X = ["스타벅스", "투썸플레이스", "이디야", "할리스", "메가커피"]
y = [starbucks_rate, twosome_rate, ediya_rate, hollys_rate, mega_rate]

plt.figure(figsize=(12, 12))
plt.title("주요 5대 커피브랜드 서울 입점 비율", fontdict={"fontsize" : 20})
plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
sns.barplot(x=X, y=y)
plt.savefig("coffee_barplot.png")
plt.show()
```
> 시각화는 seaborn을 이용하여 각자 더 멋있게! 보기 쉽게 가공할 수 있습니다.

---
# 마치며

이번 포스팅에선 지금까지 배운 내용을 바탕으로 실제 데이터를 가공하여 의미있는 결과를 얻어보았습니다. 이 외에도 다양한 데이터가 존재하고 이러한 데이터를 가공하는 연습을 하다보면 점점 숙달 될 것입니다.