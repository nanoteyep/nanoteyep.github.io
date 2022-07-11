---
layout: post
title:  "Machine Learning with Graph - 1. Why Graph?"
date:   2022-07-07
use_math: true
comments: true
categories: GNN
tags: GNN
---
# Why Graph?

---

## 시작하기에 앞서

이번 포스팅은 **Graph Neural Network**, GNN, 즉 Graph를 이용한 신경망에 대해 작성할 것 입니다.

Stanford의 강의 **CS224W**를 기반으로 작성되며 강의 내용을 정리하는 식으로 포스팅을 진행 할 생각 입니다.

---

## 1. 왜 그래프인가?

![why_graph_1](/img/why_graph_1.png)

![why_graph_2](/img/why_graph_2.png)

![why_graph_3](/img/why_graph_3.png)

* 위 그림들 처럼 많고 복잡한 자연현상들을 Relational Graph로 표현하는 것이 가능합니다.  

* 기존의 Deep Learning은 Sequence 나 Grid, 즉 텍스트나 이미지에 특화되어 있습니다만 모든 것을 Sequence나 Grid로 표현 할 수는 없습니다.

* 그렇기 때문에 그래프로 표현된 모델의 관계를 이용하여 더 좋은 예측 성능을 얻는 것이 GNN의 목표 입니다.

## 2. 왜 Graph Deep Learning이 어려운가?

![why_graph_4](/img/why_graph_4.png)

* 기존의 Text와 Image를 학습하는 모델과는 다르게 Network가 복잡하기 때문입니다.

* 이를 고정된 노드 순서가 없다고 표현합니다.

## 3. GNN으로 해결 가능한 다양한 task

![why_graph_5](/img/why_graph_5.png)

GNN은 다양한 Level의 task들이 존재합니다.  
Node level task를 활용하여 의약 분야에서 주로 활용하는 Protein folding을 예측하기도 하며  
Edege level task를 활용하여 추천시스템을 구성할 수도 있으며  
Subgraph level task를 활용하여 교통 상황 분석, 예측을 할 수 도 있으며
Graph level task를 활용하여 신약을 발견, 예측하거나 node와 edge를 정의하여 발전된 물리 시뮬레이션으로 활용을 기대 할 수도 있습니다.  

자세한 내용들은 추후에 다룰 예정이며 우선은 Graph를 활용한 ML task가 다양하게 정의 될 수 있음을 인지하고 있으면 합니다.

---
# 마치며
이번 포스팅에서는 왜 요즘 Graph가 ML분야에서 HOT한지, 무었을 가능한지 에 대해 알아보았습니다.  
CS224W의 1강과 2강에 해당하는 내용이며 특별히 중요한 내용은 아직 나오지 않았기에 최대한 간략하게 설명해 보았습니다.