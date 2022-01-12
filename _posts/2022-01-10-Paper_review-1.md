---
layout: post
title:  "Paper review - Recent Trends in Deep Learning Based Natural Language Processing"
date:   2022-01-10
categories: PaperReview
tags: NLP datascience
---
# Recent Trends in Deep Learning Based Natural Language Processing
---

## 개요

이번 포스팅은 Paper Review 포스팅 입니다.  
읽고자 하는 논문 리스트는 아래 고려대학교 DSBA lab에서 작성한 리스트를 사용하고 있습니다.  
<https://www.notion.so/c3b3474d18ef4304b23ea360367a5137?v=5d763ad5773f44eb950f49de7d7671bd>

---

## Paper

이번에 review하는 논문은 아래와 같습니다.  
<https://arxiv.org/abs/1708.02709>

첫 Paper review로는 연구의 전체적인 흐름을 정리한 review paper가 가장 적합하다고 생각하여 이번 논문을 선택하게 되었습니다.

---

## Abstract

이번 논문은 Review paper로 논문이 작성된 2017년을 기준으로 딥러닝 기반 NLP의 최신 동향을 정리해 놓았습니다.  

---

## Introduction

우선 NLP란 사람의 언어를 자동으로 분석하거나 표현하는 이론 기반의 컴퓨터 기술입니다.  
몇십년 동안 NLP분야는 SVM이나 Logistic regression과 같은 Machine learning기반의 접근 방식을 사용하였지만 neural network기반의 deep learning이 더욱 뛰어난 성능을 보여주었고 기존의 사람의 손을 거쳐야하는 features에 비해 deep learning방식은 **Word Embedding**을 시작으로 자동으로 features를 표현해 주었습니다.  

이번 paper에서는 NLP에서 주로 사용되는 **CNN**, **RNN**, **Recursive neural network**과 같은 모델은 물론 **memory-augmenting stragies**, **attention**, **unsurpervied model**, **reinforcement learning**, **deep generative model**과 같은 방식들이 어떻게 작동하는지 또한 review할 생각입니다.

이번 paper의 목차는 다음과 같습니다.

* distributed representation
* 주로 사용되는 모델
* unsupervied learning과 reinforcement learning에 의한 최신 접근방식
* memory module 과 같은 최신 트렌드
* 요약 및 성능비교

---

## Distributed representation

초창기 NLP는 **차원의 저주** 문제에 무척 취약했습니다.  
그렇기 때문에 단어를 좀 더 낮은 차원으로 표현하는 **Distributed representation**이 발전하였습니다.

* **Word Embedding**  
![nlp_review_1](/img/nlp_review_1.png)  
word embedding은 비슷한 문장에서 자주 쓰이는 유사한 단어를 찾는 것 입니다.  
즉, 주변 단어를 이용하여 단어의 특징을 표현하는 것 입니다.  
이러한 word embedding은 단어 사이의 유사성을 잘 파악합니다.  
그렇기 때문에 word embedding은 보통 데이터의 preprocessing의 첫 layer에 등장하며 주로 unlabled된 corpus로 pre-trained하고 있습니다.  
![nlp_review_2](/img/nlp_review_2.png)  
word embedding의 흥미로운 점은 위와 같이 word embedding된 두 단어사이의 연산이 의미를 연산한 것과 같은 결과가 나온다는 점 입니다.

* **Word2vec**
