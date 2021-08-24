---
layout: post
title:  "Road to Datascientist - 23. Naive bayes classifier"
date:   2021-08-23
use_math: true
comments: true
categories: DataScience python ML
---
# Naive bayes classifier

---

# 1. Naive bayes classifier란?

![naive_1](/img/naive_1.png)

> `Naive bayes classifier`는 기본적으로 분류모델로서 간단하게 설명하면 위와 같이 과연 Sunny 하고 Normal하면 테니스를 칠 것인가를 분류하는 모델입니다.

> 현재는 Y가 테니스를 친다, 안친다의 이분 구조를 가지고 있으나 여러개로 분류되어도 상관 없으며 X, 설명변수 또한 범주형이 아닌 연속형일 경우도 사용할 수 있습니다.

> ***기본적인 개념은 Y의 범주를 Y1, Y2, Y3, ... Yn이라고 가정할 때 각각의 Y가 출현 할 수 있는 확률을 구하여 가장 높은 확률로 분류하는 모델입니다.***

---

# 2. 베이즈 정리

* **조건부 확률**

![naive_2](/img/naive_2.png)

> Naive bayes classifier를 이해하기 위해서는 베이즈(bayes) 정리를 이해하여야 합니다.

> 하지만 그 전에 조건부 확률의 개념을 먼저 살펴보려고 합니다.

> 위의 식과 같이 B가 주어졌을 경우 A가 발생할 조건부 확률은 A와 B의 교집합의 확률을 B의 확률로 나눈것이며 위의 식과 같이 표현합니다.

* **베이즈(bayes)정리**

![naive_3](/img/naive_3.png)

> 베이즈 정리의 식은 위와 같으나 두가지 가정이 붙습니다.

> 첫번째, A끼리는 서로 *배반사건* 일 것

> 두번째, A를 모두 합할경우 전체가 될 것 입니다.

> A의 합은 전체이기 때문에 분모, 즉 B와 An의 교집합의 확률의 합은 결국 B의 확률이 될 것 입니다.

> 여기서 우리는 B를 입력변수 X, A를 출력변수 Y로 두고 생각한다면 분모는 Y1이라는 분류가 될 확률이며 분자만이 Y1상황에서 X값들이 만족할 확률을 나타내게 됩니다.

![naive_4](/img/naive_4.png)
![naive_5](/img/naive_5.png)

> 다시한번 테니스를 친다 와 비교해 본다면 분모는 Sunny 하며 Normal할 확률이며 분자만이 이러한 상황에서 테니스를 치는 확률과 치지 않을 확률로 구분하는 기준이 되게 됩니다.

> 또한 A는 배반사건이기 때문에 Sunny 하며 Normal한 확률은 Sunny한 확률 * Normal한 확률입니다.

> 테니스 예제의 경우 Y가 테니스를 친다, 안친다의 이분적인 분류이지만 Y가 여러개의 category를 가지는 경우 각 category별 확률이 나타납니다.

![naive_6](/img/naive_6.png)
![naive_7](/img/naive_7.png)

> Naive bayes classifier는 이렇게 분자의 확률을 계산하여 확률이 큰 YES를 $\hat y$로 결정하게 됩니다.

> 아래 식은 이 과정을 식으로 정리한 것 입니다.

> 대문자 $\pi$ 는 모든 요소의 곱을 의미하며 이 경우 X가 배반사건이기에 특정 X를 만족하는 확률을 의미합니다.

---

# 3. Naive bayes classifier의 종류

* **Gaussian naive bayes classifier**

> 설명변수, 즉 X가 연속형(numerical)할 경우 사용합니다.

> 이때의 확률은 정규분포를 따르게 되며 아래와 같이 정리 할 수 있습니다.

![naive_8](/img/naive_8.png)
![naive_9](/img/naive_9.png)

* **Multinomial naive bayes classifer**

> 설명변수, 즉 X가 범주형(categorical)일 경우 사용합니다.

> 범주형 이지만 이분형일 경우 **Bernoulli(베르누이) bayes classifier**라고 합니다.

> 마찬가지로 이때의 확률분포는 다음과 같습니다.

![naive_10](/img/naive_10.png)
![naive_11](/img/naive_11.png)

---

# 4. 정리

* naive bayes classifier의 경우 입력변수 X들 간의 상관관계가 존재하지 않는다는 가정하에 시행되는 모델입니다.

* 또한 간단한 기술로 만들어져 있어 적은 데이터 양으로도 복잡한 실제상황에서도 잘 작동한다는 장점이 있습니다.

* 하지만 가장 처음 서술하였듯이 features간에 상관관계가 없을 경우를 가정하기 때문에 사용하지 못할 경우가 많습니다.

---
# 마치며
이번 포스팅에서는 분류 모델인 naive bayses classifier에 대하여 알아보았습니다. 다음 포스팅 부터는 이번 포스팅과 마찬가지로 모델과 그와 관련된 수학적 개념을 동시에 다뤄보려고 합니다.