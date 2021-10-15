---
layout: post
title:  "Road to Datascientist - 29. Deep Learning - Neural Network"
date:   2021-09-16
use_math: true
comments: true
categories: DataScience
tags: DataScience DL
---
# Deep Learning__Neural Network__

---

# 1. Deep Learning 개요

## 1.1 시작하기에 앞서

* `Deep Learning`을 배우기 앞서 전에 포스팅 하였던 `Neural Network`에 관한 내용을 다시 한번 되새겨 보는 것을 추천합니다.

> <https://nanoteyep.github.io/datascience/python/ml/2021/08/27/Road-to-DataScientist_31.html>

* 특히 핵심 토대가 되는 내용을 설명 하면서 위 포스팅에서 다루었던 부분에 대하여 다시한번 다루게 될 것 입니다.

## 1.2 개요

* `Deep Learning`은 `Machine Learning`에 한 종류인 `Neural Network`에 기반하여 많은 양의 데이터, 즉 사진 혹은 동영상과 같은 거대한 인풋, 을 학습하여 뛰어난 성능을 이끌어 내는 분야입니다.

* `Classification(분류)` 와 `Regression(회귀)` 가 주 목적인 `Machine Learning` 과는 다르게 `Deep Learning`은 `Machine Learning`의 기능은 물론 물체 검출, 영상 분할, 저 해상도 복원, 예술행위, 강화학습 등 다양한 응용이 가능합니다.

![deep_1](/img/deep_1.png)

* `Deep Learning`의 구성요소는 위와 같으며 `네트워크 구조`를 구하는 과정이 학습에 해당합니다.

---

# 2. Neural Network 기본

## 2.1 Perceptron

![deep_2](/img/deep_2.png)

* `Neural Netwrok` 포스팅에서 설명하였듯이 `Perceptron`은 신경세포를 모방한 위 그림과 같은 구조 단위를 뜻합니다.

## 2.2 Activation function(활성 함수)

* `Activation function`은 `Perceptron` 출력에 의미를 부여해주는 함수 입니다.

* **sign 함수**, **tanh 함수**, **sigmoid 함수**, **softmax 함수**, **ReLU** 등의 종류가 있습니다.

* 딥러닝에서 가장 자주 사용하는 함수는 당연 **ReLU**이며 *softmax* 는 여러 경우의 출력 값 중 하나에 속할 확률로 표현 해주는 함수입니다. 그렇기에 최종 출력단계에서 여러가지 범주로 분류해주는 Multi-class classification에서 자주 사용합니다.

## 2.3 Loss function(손실 함수)

* 손실 함수는 *학습 중* **알고리즘이 얼마나 못 하는지** 를 표현합니다.

* 그렇기 때문에 *학습 후* **알고리즘을 비교, 평가** 하기 위한 ***성능 척도*** 와 구분됩니다.

* `Deep Learning`은 이 손실 함수를 최소화 하는 방향으로 학습하며 보통 미분 가능한 함수를 사용, 미분값을 이용해 최솟 값을 구하게 됩니다.

* 회귀 모델에서 자주쓰이는 **MSE**, **MAE**가 있으며 분류 모델에서는 **Cross Entropy Error(CEE)** 가 주로 사용됩니다.

## 2.4 Fully connected layer

![deep_3](/img/deep_3.png)

* 두 Layer(계층)간의 모든 뉴런이 연결되어 있는 계층을 뜻합니다.

## 2.5 Shallow Neural Network

![deep_4](/img/deep_4.png)

* Deep Neural Network 등장 이전의 기존 신경망을 뜻합니다.

* 각 계층이 `Fully connected layer`로 되어 있으며 `input`, `hiddem`, `output`으로 구성되어 있습니다.

## 2.6 Deep Neural Network

![deep_5](/img/deep_5.png)

* `Shallow neural network`보다 `hidden layer`가 많은 신경망을 뜻합니다.

* 보통 5개 이상의 계층이 있는 경우 Deep 하다고 표현합니다.

## 2.7 Gradient discent(경사 하강법)

![deep_6](/img/deep_6.png)

* 경사 하강법은 `Deep Learning`에서 손실 함수의 최솟값을 찾기위해 사용됩니다.

* 단순 미분법으로 찾게 된다면 local minimum에 도달할 가능성이 높으며 이를 극복하기 위한 첫번째 방법입니다.

![deep_7](/img/deep_7.png)

* 적절한 학습률을 설정하는 것이 중요하며 학습률이 작은경우 local minimum을 탈출하지 못하며 시간이 오래걸릴 수 있고 너무 크다면 minimum을 제대로 찾지 못할 수 있습니다.

### 2.7.1 local minimum을 탈출하는 방법

* **Momentum(관성)**

![deep_8](/img/deep_8.png)

> 관성계수와 이동벡터를 이용하여 Local minimum과 잡음에 대처 할 수 있습니다.

> 이동벡터를 추가로 사용하기 때문에 경사 하강법에 비해 2배의 메모리를 차지합니다.

* **AdaGrad(적응적 기울기)**

![deep_9](/img/deep_9.png)

> Adaptive Gradient의 줄임으로 변수별로 학습률이 달라지게 하는 알고리즘 입니다.

> g는 누적되므로 학습이 많이 되면 점차 커지게 되고 이는 학습률을 감소시켜 학습이 많이 되지 않은 다른 변수들이 잘 학습될 수 있도록 합니다.

> 학습이 오래된다면 g가 계속 커져 더이상 학습이 이루어지지 않는 단점이 있습니다.

* **RMSProp**

![deep_10](/img/deep_10.png)

> AdaGrad의 단점을 개선한 방법으로 합 대신 지수평균을 사용합니다.

* **Adam**

![deep_11](/img/deep_11.png)

> Adaptive moment estimation(Adam) 은 RMSProp와 Momentum의 장점을 결합한 알고리즘입니다.

> 현재 가장 **최신 기술**이며 딥러닝에서 **가장 많이 사용**하고 있습니다.


## 2.8 Back Propagation(역전파 알고리즘)

![deep_12](/img/deep_12.png)

> `Back propagation`은 `Gradient descent`한 스텝을 계산하는데 너무 많은 연산이 필요하기 때문에 개발되었습니다.

> 겉미분과 속미분 으로 알려진 미분의 연쇄법칙을 이용하면 신경망을 각 layer를 하나의 함수로 하는 합성함수로 표현 할 수 있습니다.

![deep_13](/img/deep_13.png)
![deep_14](/img/deep_14.png)

> 합성함수의 미분을 진행하면 점점 이전 연산의 미분값을 참조하기 때문에 점차 뒤로 전파되는 것으로 표현됩니다.

![deep_15](/img/deep_15.png)

> 실제 연산에서는 위와 같은 트리에 연산 과정을 저장하고 이를 뒤로 미분하면서 연산하게 됩니다.

> 이와 같은 `Back Propagation`은 `Gradient descent`를 여러번 하는 것에 비해 연산 복잡도를 획기적으로 줄여주었습니다.

## 2.9 Gradient vanishing(기울기 소실)

* `Back propagation`은 미분을 사용하기 때문에 `activation function`으로 미분하기 쉬운 `sigmoid function`을 사용하고 있었습니다.

* 그러던 와중 layer가 많아지게 되면 `sigmoid function`의 문제로 기울기가 사라지는 문제가 있었습니다.

* 현재는 `ReLU`를 `activation function`으로 사용하면서 이 문제를 해결하였으며 자세한 내용은 `Neural Network` 포스팅에 있습니다.

---
# 마치며
이번 포스팅에서는 본격적으로 `Deep Learning`에 대해 다루기 전에 토대가 되는 `Neural Network`에 대해 다시한번 알아보았습니다. 비록 전의 포스팅과 겹치는 부분이 많고 보다 간략하게 설명하였으나 앞으로 알아볼 다른 딥러닝을 이해하기 위한 필수적인 개념이기 때문에 다시한번 점검해 보았습니다.