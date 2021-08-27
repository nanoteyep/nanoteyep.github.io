---
layout: post
title:  "Road to Datascientist - 27. 신경망 모형"
date:   2021-08-27
use_math: true
comments: true
categories: DataScience python ML
---
# Neural Network

---

# 1. 신경망 모형(Neural Network)이란?

* 신경망 모형은 인간의 신경세포, 즉 뉴런의 구조를 모델링 하는 학습입니다.
* 하나의 뉴런을 `perceptron` 이라고 표현하며 B에 해당하는 부분입니다.
* 또한 이런 여러개의 `perceptron`을 합쳐 여러개의 layer를 구성함으로서 뉴런의 결합인 synapse를 모델링합니다.
![nn_1](/img/nn_1.png)

* **Perceptron**

![nn_2](/img/nn_2.png)

![nn_4](/img/nn_4.png)

> `perceptron`은 하나의 뉴런을 구조화 시킨 모델입니다.

> 입력데이터 X를 받으며 이에 대한 `weights`, `weights`는 선형회귀 모델에서의 $\beta$와 같은 역할을 합니다.

> `input`과 `weights`를 계산하여 결과값을 내며 이것을 `activation function`에 적용 시킨 후 또 다른 `perceptron`의 `input`값으로서 적용시킵니다.

* **activation function**

![nn_5](/img/nn_5.png)

> `activation function`은 `hidden layer`를 의미있게 쌓아주는 역할을 합니다.

> `step`방식과 `sigmoid`방식은 초창기 `neural network`에서 많이 쓰이던 `activation function`으로 몇가지 문제점을 안고 있습니다만 추후에 설명하도록 하겠습니다.

* **Multi Layer Perceptron**

![nn_3](/img/nn_3.png)


> `perceptron`이 하나의 뉴런을 의미한다면 `Multi Layer Perceptron`은 뉴런의 결합, synapse를 의미합니다.

> 위 그림에서는 하나의 `Hidden layer`가 존재하며 이는 4개의 `perceptron`으로 이루어져 있습니다.

> 또한 그렇게 나온 `Output layer`는 총 2개의 `perceptron`으로 이루어져 있습니다.

* **Playground tensorflow**

> [Playground_tensorflow](https://playground.tensorflow.org/#activation=tanh&batchSize=10&dataset=circle&regDataset=reg-plane&learningRate=0.03&regularizationRate=0&noise=0&networkShape=4,2&seed=0.86254&showTestData=false&discretize=false&percTrainData=50&x=true&y=true&xTimesY=false&xSquared=false&ySquared=false&cosX=false&sinX=false&cosY=false&sinY=false&collectStats=false&problem=classification&initZero=false&hideText=false)
    
> nerual network를 이해하는데 많은 도움을 주는 사이트 입니다.

> 직접 간단하게 `perceptron`을 추가하고 조정하며 모델을 돌려볼 수 있습니다.

---

# 2.