---
layout: post
title:  "Road to Datascientist - 33. Deep Learning - National Language Processing"
date:   2021-10-08
use_math: true
comments: true
categories: DataScience 
tags: DataScience DL
---
# Deep Learning - NLP

---

# 1. NLP란?

* NLP는 National Language Processing의 줄임말로 자연어 처리 라고도 부릅니다.

* 이 전에 배웠던 RNN기법을 사용하여 **INPUT DATA**로서 **자연어, 즉 인간이 사용하는 언어단계**를 입력하고 출력합니다.

* 학습을 통해 무엇을 산출하는 작업인가를 **downstream task**라고 부르며 *질문과 응답*, *번역*, *텍스트 생성*, *텍스트 요약*, *텍스트 유사도 검증* 등 다양한 작업을 할 수 있습니다.

---

## 2. NLP pipeline

* **데이터 수집**
* **데이터 전처리**
* **임베딩**
* **downstream task**
* **prediction**

> NLP는 위와 같은 과정을 순서대로 거치며 진행됩니다.

## 2.1 데이터 수집

**학습에 사용될 데이터를 수집하는 단계 입니다.**

* 구입
* 크롤링

> 크게 위의 두가지 방법으로 수집하며 데이터를 구입하는 경우 많은 시간과 노력을 아낄 수 있지만 전체적으로 데이터 양이 부족하며 비용이 발생하게 됩니다.

> 크롤링의 경우 직접 인터넷 상에서 자연어 정보를 수집하는 방법으로 전처리 과정에서 정말로 많은 시간이 소요되지만 노력한 만큼 방대하고 저렴한 데이터를 얻을 수 있습니다.

이렇게 자연어처리를 위한 문장들로 구성된 데이터 셋을 **Corpus**라고 합니다.

## 2.2 데이터 전처리

**학습에 앞서 데이터를 가공하는 단계입니다. 성능에 영향을 미치는 가장 중요한 단계이기도 합니다.**

* 정제
* Labeling
* Tokenization

> 크게 위의 세가지 단계로 이루어져 있으며 Labeling의 경우 downstram task에 따라 생략 할 수 있습니다.

> **정제**는 downstream task에 맞게 필요한 데이터와 필요없는 데이터를 구분하여 지워주거나 합쳐주는 단계입니다.

> 예시로 text를 음성으로 출력하는 task의 경우 특수문자나 기호, 이모티콘 등은 출력 할 수 없기 때문에 이 단계에서 미리 지워주는 **정제**과정을 거칩니다.

> 혹은 <은, 는, 이, 가>를 포함한 단어 등을 같은 의미를 가질 수 있도록 합치는 과정또한 **정제**에 포함됩니다.

> **Labeling**은 문장마다, 혹은 단어마다 task에 걸맞는 label을 생성하는 단계입니다.

> 예시로 유사도 검증을 위해서는 유사도를 비교하기 위한 한쌍의 문장이 유사한지, 유사한 문장이 아닌지 label을 씌워주어야 합니다.

> 이 단계는 성능을 검증하는 것과 밀접한 관계가 있으므로 아무리 오래걸린다 하여도 사람이 직접 하는 경우가 많습니다.

> **Tokenization**은 문장을 텍스트 분석단위로 나누어 주는 단계입니다.

> 이번 학습을 글자 단위로 학습한다면 글자 단위로, 형태소 단위로 학습한다면 형태소 단위로, 단어 단위로 학습한다면 단어 단위로 분해합니다. 물론 더 큰 단위로 분해 할 수도 있습니다.

> 이 때, 분해하는 단위가 크면 클수록 OoV(Out of Value),사전에 존재하지 않는 단어, 가 증가하여 희소성 문제가 증가하지만 Sequence가 짧아져 모델의 부담이 작아집니다. 때문에 이를 적절히 잘 설정하여야 합니다.

> 한글의 경우 **Mecab**, **Konlpy**등과 같은 형태소 분석기를 사용 할 수 있습니다.

## 2.3 임베딩

**학습을 위해 텍스트를 벡터로 변환하는 과정입니다.**

* Vector Space Model
* Neural Network based Language Model(NNLM)

> 크게 위의 두가지로 분류됩니다.

> **Vector Space Model**은 **Tokenization**된 최소단위인 **Token**을 기준으로 전체 **Documents**의 **Token**을 모아둔 **사전**을 전체 차원으로 가지고 특정 Token이 존재하면 1 존재하지 않는다면 0으로 변환한 **one-hot encoded Vector**를 반환합니다.

> 하지만 이 특정 단어의 중요도와 같은 정보를 포함하지 않습니다.

> 그렇기에 조금 더 발전된 **TF-IDF**모델이 존재합니다.

> **TF-IDF**는 **특정 Document**에서 **해당 Token**이 등장하는 횟수, TF를 **해당 Token**이 존재하는 **모든 Documents**, DF로 나누어준 값을 1대신 채워주는 모델입니다.

> **Vector Space Model**은 차원이 너무 크기 때문에 *PCA*, *Auto Encoder* 등을 사용하여 차원을 낮춰주곤 합니다.

> **NNLM**은 Neural Network를 사용한 임베딩 방법 입니다.

> **Word embedding**, **Sentence embedding**과 같은 방식이 존재하며 *Word to Vector*, *Glove*, *BERT* 등과 같은 모델이 존재합니다.

> **Word embedding**의 원리를 간단히 설명한다면 *같은 위치에 존재하는 단어는 유사한 의미를 가진다* 라는 방식을 이용하여 word vector를 축소하는 방법입니다.

> 간단하게 Auto Encoder와 같은 원리를 가지고 있습니다.


## 2.4 Downstream task

**목적에 따라 모델을 설정하고 학습하는 단계입니다.**

> 대부분은 PLM(Pretrained LM)을 기반으로 미리 만들어진 모델을 사용합니다.

> 그 이유는 NLP가 개인이 학습시키기에는 너무 방대한 비용이 들기 때문에 Google과 같은 대형 기업에서 공개한 PLM을 사용합니다.

> 이 때 사용하는 모델은 어떤 task인지에 따라 조금 씩 변화 합니다.

## 2.5 Prediction

**모델에 넣고 추론을 진행합니다.**

---

# 마치며
이번 포스팅에서는 NLP에 대해 알아보았습니다. NLP는 downstream task에 따라 전처리 과정, 임베딩 과정이 상이하기 때문에 목적에 맞게 데이터를 가공하는 것이 가장 중요합니다. 그 과정에서 단순한 ML을 이용하는 **vector space model**을 사용 할 수도 있고 NN을 이용하는 **NNLM**을 이용 할 수도 있습니다. 물론 성능은 NNLM이 훨씬 뛰어납니다.