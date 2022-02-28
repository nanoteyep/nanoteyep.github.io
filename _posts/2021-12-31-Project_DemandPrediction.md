---
layout: post
title:  "Project - 수요 예측"
date:   2021-12-31
categories: Project
tags: DataScience PyTorch DL
---
# Demand Prediction
---

## 시작하기에 앞서

* 이번 포스팅은 2021년 12월 31일 당시 모 기업의 `Coding test`의 일환으로 진행한 기업 데이터를 활용한 **수요 예측** 프로젝트 입니다.

* 기업측에서 데이터와 문제의 공유를 금지하였기 때문에 데이터를 탐색하는 부분과 문제 상세를 제외하였습니다.

* 사용된 노트북은 아래 공개해 두었습니다.
> <https://github.com/nanoteyep/ToyProject/blob/main/DemandPrediction/Demand_prediction.ipynb>  

---

## 문제 정의

문제의 상세정보를 게시 할 수는 없으니 간략하게 정의하자면 실제 기업 데이터를 활용하여 수요를 예측하는 문제입니다.

---

# 모델 설명

모델은 **LSTM**을 활용하였습니다.  
과제의 특성상 딥러닝 모델을 사용해야 했으며 데이터가 시간에 기반한점, 그리고 timestep이 길었기 때문입니다.  

모델은 7개의 timestep이 순서대로 입력되며 1개의 output을 발생합니다.  
그렇게 생성된 output은 마지막 timestep으로 작용하여 한 step 진행한 7개의 timestep이 다시 순서대로 다음번 input이 되어 또 다시 한개의 output을 생성합니다.  
이렇게 생성된 값을 `MSE`를 사용하여 모델의 성능을 높이는 방향으로 진행하였습니다.


---
# 마치며

![demand_1](/img/demand_1.png)

공유해둔 노트북에서도 확인 할 수 있지만 epoch가 진행됨에 따라 오차또한 점점 줄어드는 것을 확인 할 수 있습니다.  
당시에는 제출 기한이 다가와 오차를 더 줄일수 있음에도 불구하고 학습을 더 진행하지 못한 점이 아쉽게 다가왔습니다.  
또한 과제의 결과값으로 MSE와 MAE를 요구하였기에 이 둘만 측정하였지만 MAPE를 측정하여 좀 더 알기 쉬운 오차율을 만들어 표시해야 했다고 생각합니다.  
