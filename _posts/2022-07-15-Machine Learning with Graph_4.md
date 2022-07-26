---
layout: post
title:  "Machine Learning with Graph - 4. Link Prediction tasks and Features"
date:   2022-07-15
use_math: true
comments: true
categories: GNN
tags: GNN
---
# Link Prediction tasks and Features


---

## 1. Link-level Prediction 이란?  

기존의 Link에 기반해서 새로운 Link를 예측 하는 것을 의미합니다.  
새로운 Link를 예측하는 방법에는 크게 두가지가 있습니다.  

* **Random LInk를 지우고 학습하는 방법**  
* **시간이 지나면서 생기는 Link를 예측하는 방법**  

첫 번째 방법은 Protein구조 와 같이 이미 충분히 복잡하게 얽혀져 더이상 graph의 변화를 기대하기 힘든 경우 사용합니다.  
두번째 방법은 시간이 지남에 따라 link상태가 변하는 인간관계 network 등에서 사용합니다.  

전체 graph에서 두 Node pair (x,y)를 뽑아 두 Node관의 score c(x,y)를 계산하고 실제로 Node간에 Link가 생겼는지 관찰합니다.

## 2. Link-level Features



