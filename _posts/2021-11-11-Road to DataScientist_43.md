---
layout: post
title:  "Road to Datascientist - 39. Deep Learning - National Language Processing - Huggingface Transformers"
date:   2021-11-11
use_math: true
comments: true
categories: DataScience 
tags: DataScience DL NLP PLMs HuggingFace
---
# Deep Learning - NLP__HuggingFace Transformers

---

# 시작하기 앞서

이번 포스팅에서는 **BERT**를 이용해 **Text Classification**, 즉 감정분석 코드를 작성하고 리뷰하는 형식으로 설명하겠습니다.  
코드는 kh-kim님의 simple-ntc를 활용하겠습니다.

> <https://github.com/kh-kim/simple-ntc>

repo안에는 다양한 파일이 존재하지만 이번 포스팅에서는 다음과 같은 파일만 사용하도록 하겠습니다.

![hug_1](/img/hug_1.png)

---

# 1. HuggingFace Transformers란

* **HuggingFace Transformers**란 Transformers의 **NLG**, **NLU** 를 위한 **General-purpose 아키텍처**를 제공합니다.  
**BERT**, **RoBERT**, **GPT**등을 말하며 PyTorch 및 TensorFlow에서 모두 동작합니다.

* **Model hub**를 제공합니다.  
사용자 끼리 모델을 공유하고 통일된 플랫폼 위에서 작업할 수 있게 해줍니다.  
사전학습된 모델을 단순히 아이디 하나로 자동 다운로드 및 학습이 가능합니다.

* 뿐만아니라 **전처리, 학습, 벤치마크 테스트 코드**까지 모두 제공합니다.  
때문에 전체 pipeline이 쉽게 통합 될 수 있습니다.

* 또한 **Documentation**이 무척 잘되어있어 쉽게 코드에 대한 설명을 볼 수 있습니다.  
> <https://huggingface.co/transformers/>

---

# 2. bert_dataset.py


PyTorch를 이용하여 Downstream task를 하기 위해서는 가장 먼저 **DataSet** 과 **DataLoader**가 필요합니다.

**DataSet**에서 **Data를 정의하고**, **Data의 길이, 크기를 알며**, **미니배치를 위한 샘플반환** 역할을 수행합니다.

**DataLoader**는 설정된 미니배치만큼 데이터를 불러오는 역할을 합니다.

## 2.1 TextClassificationDataset()

PyTorch로 custom dataset을 구성하기 위해서는 dataset class를 생성하고 이 class는 총 3개의 메소드가 필요합니다.

* **__init__**  
데이터를 정의하는 메소드입니다. class를 선언할 때 text와 label을 parameter로 받습니다.  
미니배치를 설정하지 않은 전체 dataset을 구성하는 메소드 입니다.

* **__len__**
데이터의 크기를 정의하는 메소드입니다. 실제로 text의 길이를 측정하여 return합니다.
DataLoader로 데이터를 불러오기 위해 필요한 메소드 입니다.

* **__getittem__**
주어진 index의 data를 불러오는 메소드입니다.
DataLoader가 미니배치를 생성하여 데이터를 불러오는데 활용되는 메소드 입니다.

## 2.2 TextClassificationCollator()

NLP는 자연어를 다루기 때문에 각 문장마다 길이가 전부 다릅니다.  
하지만 이를 학습에 응용하고자 하면 문장의 길이가 전부 다르기 때문에 Tensor의 크기도 전부 다르게 됩니다.

이를 해결하기 위해 **collate_fn**을 정의합니다.

TextClassificationCollator() 클래스는 **DataLoader**의 **collate_fn**으로 사용될 것 이며 미니배치 내의 문장을 리스트 형식으로 받아 가장 긴 문장을 기준으로 **padding**을 생성하는 역할을 수행합니다.

* **__init__**
tokenizer, max_length와 with_text를 입력받아 클래스 내의 변수로 지정합니다.
사용된 tokenizer와 dataset내의 문장에서 max_length를 입력받아 넣어주면 됩니다.

* **__cal__**
본격적으로 collate_fn으로 작용하는 메소드 입니다.
samples라는 이름으로 받아들인 문장과 lables를 리스트로 저장하고 tokenizer의 padding기능을 이용하여 **return_value**라는 dictionary를 생성합니다.
**return_value**는 기본적으로 tokenizer가 생성한 token의 index, attention mask, label을 저장합니다.
with_text가 True일 경우 본문의 text또한 이 dictionary에 저장합니다.

## 2.3 실제 적용

위의 class들은 실제 **finetune_plm_hftrainer.py**에서 다음과 같이 사용되었습니다.

```python

train_dataset = TextClassificationDataset(texts[:idx], labels[:idx])
valid_dataset = TextClassificationDataset(texts[idx:], labels[idx:])

# --------------------------------------------------------------------------

trainer = Trainer(
    model=model,
    args=training_args,
    data_collator=TextClassificationCollator(tokenizer,
                                   config.max_length,
                                   with_text=False),
    train_dataset=train_dataset,
    eval_dataset=valid_dataset,
    compute_metrics=compute_metrics,
)
```

---

# 3.  Tokenizor

HuggingFace를 사용한다면 사람들이 공유해둔 Tokenizor를 간단히 학습에 이용할 수 있습니다.

아래는 실제 **finetune_plm_hftrainer.py**에서 사용된 부분을 추출한 것 입니다.

```python

from transformers import BertTokenizerFast

tokenizer = BertTokenizerFast.from_pretrained(config.pretrained_model_name)
```

config는 아래 함수에서 정의되어 있습니다.

![hug_2](/img/hug_2.png)

이와 같이 HuggingFace는 Pre-trained된 언어모델을 간단하게 코드 3줄로 작성할 수 있습니다.

---

# 4. finetune_plm_hftrainer.py

이번 학습의 **main**을 담당하는 파일입니다. 실제로 main() 함수에서 모든 진행상황을 다루고 있습니다.  

library import 부분부터 차근차근 뜯어 보도록 하겠습니다.

## 4.1 Library