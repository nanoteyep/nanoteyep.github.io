---
layout: post
title:  "Road to Datascientist - 31. Deep Learning - Recurrent Neural Network"
date:   2021-10-05
use_math: true
comments: true
categories: DataScience
tags: DataScience DL
---
# Deep Learning - RNN

---

# 1. RNN이란?

* RNN은 `Recurrent Neural Network`의 약자로 주로 **Sequencial Data**, 즉 순서에 의미가 있어 순서가 달라질 경우 의미가 손상되는 순차데이터를 다룰 때 주로 사용합니다.

* 그렇기 때문에 입력을 순차적으로 받아 그 내용을 **기억(Memory)** 할 수 있어야 하며 이를 위하여 **순환(Recurrent)** 이 적용됩니다.

* 주로 사용되는 영역은 비디오, 자연어 등이 있습니다.

## 1.1 Vanilla RNN

![rnn_1](/img/rnn_1.png)

> 기존의 Shallow Neural Network에 **Recurrent(순환)** 이 추가되었으며 출력이 이전의 모든 입력에 영향을 받습니다.

## 1.2 Process Sequence

![rnn_2](/img/rnn_2.png)

> RNN의 **Computational Graph(구조도)** 를 살펴보면 위와 같습니다.

> 이 중, one to one은 기존의 Vanilla RNN과 같은 구조이며 데이터와 목적에 따라 다양한 입력 방식과 출력방식을 결정 할 수 있습니다.

> 예를 들어, text를 입력받아 그 다음 문자를 예측하는 모델에는 many to one, 비디오를 입력받는 모델에는 many to many가 주로 사용됩니다.

---

# 2. Back propagation

![rnn_3](/img/rnn_3.png)

> RNN의 Back propagatio은 타 모델과 비교해 조금 특이한 성질을 가지고 있습니다.

> 각 Sequence에서 사용되는 W<sub>_hh</sub>가 각각 존재하는 것이 아닌 모두 동일한 값을 사용하기 때문에 Back propagation이 진행되는 동안 계속해서 update가 이루어 집니다.

> 즉, W<sub>_xh</sub>는 W<sub>1xh</sub>, W<sub>2xh</sub> 와 같이 각각의 Sequence마다 따로 존재하지만 W<sub>_hh</sub>만큼은 모두 동일한 W<sub>_hh</sub>이게 됩니다.

> 그 이유는 input data가 Sequencial data이기 때문에 순서에 따른 정보를 소실하지 않기 위해 하나의 Weight를 이용하여야 하기 때문입니다.

![rnn_4](/img/rnn_4.png)

> 또한 backpropagation은 computationla graph에서 나타난 화살표의 반대방향으로 진행됩니다.

---

# 3. 응용

![rnn_5](/img/rnn_5.png)

> RNN을 응용한다면 위와 같은 image captioning도 가능합니다.

> CNN을 이용해 이미지를 학습하고 RNN에 집어넣어 `straw hat`이라는 키워드를 출력해 낸 것 입니다.

![rnn_6](/img/rnn_6.png)

> 좀 더 복잡한 모델을 구성하여 위와 같이 이미지를 설명하는 모델도 구성이 가능합니다.

## 3.1 LSTM

![rnn_7](/img/rnn_7.png)

> Vanilla RNN 모델은 Gradient vanishing문제에 의해 오래전에 입력된 데이터는 소실되어 학습에 반영이 제대로 되지 않는 문제가 있습니다.

> LSTM, Long Short Term Memory는 이를 보안하기 위한 모델으로 입력값과 그 전 출력값을 그대로 activation function에 집어넣는 기존의 Vanilla 모델과 달리 Cell state라는 Residual network를 가지고 있어 기억을 오랫동안 유지할 수 있습니다.

> 이 외에도 forget gate, input gate, output gate가 있어 Cell state의 값을 얼마나 잊을지, input data를 얼마나 활용할지, output data로 얼마나 비중을 둘지 sigmoid 함수를 이용해 결정 할 수 있습니다.

> 제대로 이해한다면 무척 복잡하겠지만 기본적으로 RNN과 computational graph가 같으며 CNN의 ResNet과 같이 Redidual을 활용하여 Gradient vanishing을 막는 기법이라고 생각하면 됩니다.

---
# 마치며
LSTM이외에도 GRU와 같은 기법이 존재하고 Sequence to Sequence나 attemtion과 같은 심화 개념이 존재합니다만 지금 당장은 다루지 않으려고 합니다. 특히 자연어 처리 파트를 따로 두어 이후의 RNN의 발전방향의 대부분은 자연어처리 포스팅에서 다루고자 합니다. 이번 포스팅에선 RNN의 개념에 대해 잠시 다루었으며 우선 다음 포스팅에서 Generate model 에 관해 다룬 후 자연어처리라는 주제로 다시 다루도록 하겠습니다.