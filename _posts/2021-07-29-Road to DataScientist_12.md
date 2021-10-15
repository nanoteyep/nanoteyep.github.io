---
layout: post
title:  "Road to Datascientist - 9.Web Crawling"
date:   2021-07-29
categories: DataScience 
tags: DataScience python WebCrawling
---
# Web Crawling
---

## 1.Web Crawling 이란?

* `Web Crawling`이란 웹페이지 상의 정보를 가져와 데이터로 가공하는 것을 의미합니다.
* 예를 들어 지금 현재 제 블로그의 모든 포스팅 제목을 데이터로서 저장하거나 특정 포스팅의 내용을 데이터로서 저장하는 것이 바로 `Web Crawling`입니다.

---

## 2. 개발자도구(chrome)를 이용하여 웹 페이지 분석하기

![webcrw_1](/img/webcrw_1.png)

* 오른쪽에 보이는 도구가 chrome에서 제공하는 개발자도구 입니다.
* 웹 페이지 상에서 F12를 눌러보면 확인 할 수 있습니다.
* 여러가지를 확인 할 수 있으나 이번 포스팅에서는 빨간 표시의 세가지만 알아보겠습니다.

1. 첫 번째, `요소선택` 입니다. <br>
`요소선택` 버튼을 누르고 웹 페이지에 마우스를 옮겨가며 확인하면 각 요소가 `elememt`란의 어디에 있는지 바로 보여줍니다.
 
2. 두 번째, `element`입니다. <br>
`element`는 현재 페이지의 모든 정적인 정보를 표시하고 있습니다. 쉽게 말해 지금 눈으로 보이고 있는 모든 것이 이 `element`란에 표시되고 있는 것 입니다.

3. 세 번째, `network`입니다. <br>
`network`는 만약 현재 페이지에서 어떠한 동작을 통하여 서버와의 통신이 이루어졌다면 그 요청과 응답을 기록해 놓은 란 입니다.

* 이제부터 이 chrome에서 제공하는 개발자 도구와 새롭게 배울 `python library`를 이용하여 `Web Crawling`을 하는 방법에 대해 알아보겠습니다.

---

## 3. requests 모듈

* `Web Crawling`을 위한 대표적인 모듈, `requests`입니다.

```python
import requests
```

* `requests`에 대해 설명하기 전에 `get`방식과 `post`방식에 대해 알아보도록 하겠습니다.

### 3.1 get 방식

* `get`방식은 웹 페이지의 정보를 주소에 저장하는 방식입니다.
* 예를 들어 이 전 포스팅의 주소는 `https://nanoteyep.github.io/datascience/python/2021/07/26/Road-to-DataScientist_11.html` 입니다.
* 이 주소에는 제 블로그 주소인 `nanoteyep.github.io` 와 그 세부정보인 `datascience/python/.....`가 들어있습니다.
* 이런 식으로 주소에 페이지의 정보를 담아 서버로 전송하는 방식을 `get`방식이라고 합니다.

### 3.2 post 방식

* `post`방식은 주로 로그인에 이용되는 방식입니다.
* `get`처럼 주소창에 페이지의 정보를 보내게 된다면 로그인 정보인 `ID`와 `PW`가 노출되게 될 것 입니다.
* 그렇기 때문에 `post`방식은 내부의 `Form data`에 정보를 담아 보내게 되며 이는 개발자도구의 `network`란에서 확인 할 수 있습니다.

### 3.3 requests 사용법

```python
url = 'https://nanoteyep.github.io/datascience/python/2021/07/26/Road-to-DataScientist_11.html'
resp = requests.get(url)
resp.text
```

* 위의 코드는 `get`방식으로 웹 페이지의 정보를 얻어 온 것 입니다
* `resp.text`는 페이지의 모든 `element`정보를 `str`로 출력하는 것 입니다.
* `resp`를 직접 출력해 보면 <Response [200]> 이라는 값을 얻을 수 있습니다.
* 이는 웹 페이지에 정상적으로 접속하여 응답을 얻었다는 의미이며 뒤의 숫자코드에 의해 그 상태를 알 수 있습니다.
* 이와 같은 코드를 HTTP 상태코드라 하며 다음과 같은 의미를 지닙니다.

#### HTTP 상태 코드
 - 1xx (정보): 요청을 받았으며 프로세스를 계속한다
 - 2xx (성공): 요청을 성공적으로 받았으며 인식했고 수용하였다
 - 3xx (리다이렉션): 요청 완료를 위해 추가 작업 조치가 필요하다
 - 4xx (클라이언트 오류): 요청의 문법이 잘못되었거나 요청을 처리할 수 없다
 - 5xx (서버 오류): 서버가 명백히 유효한 요청에 대해 충족을 실패했다

[출처: 위키피디아](https://ko.wikipedia.org/wiki/HTTP_%EC%83%81%ED%83%9C_%EC%BD%94%EB%93%9C)

```python
url = 'https://nanoteyep.github.io/datascience/python/2021/07/26/Road-to-DataScientist_11.html'
headers = {
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36'
}
resp = requests.get(url, header=headers)
resp.text
``` 

* 위 코드는 `get`방식에서 `header`를 추가한 것 입니다.
* `get`방식 이지만 주소에 적힌 데이터 이외에도 사용자의 브라우저 정보와 같은 기타 데이터를 추가 할 수 있습니다.

```python
python
url = 'https://nanoteyep.github.io/datascience/python/2021/07/26/Road-to-DataScientist_11.html'
data = {
    'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36'
}
resp = requests.post(url, data=data)
resp.text
```

* 이번 코드는 `post`방식입니다.
* 사실 `get`방식과 사용법 자체는 매우 유사합니다.
* 다만 `post`방식의 특징인 `Formdata`를 위와 같이 `dict`형식으로 생성하여 인자로 추가 해 줄 수 있습니다.
* 지금 예시에는 적당한 `Formdata`가 없기에 위의 `header`와 같은 값을 넣어 주었지만 `Formdata`에 들어갈 내용을 넣어주시면 됩니다.

### 3.4 response

* `response`는 웹 페이지로부터의 반응을 의미합니다.
* 위의 예시 코드에는 모두 `resp`라는 변수에 이 `response`를 저장하였으며 `.text`를 통해 `html`전문을 출력하고 있습니다.
* `response`는 `.text`이외에도 다양한 방식으로 확인 할 수 있으며 이에 관해선 이번 포스팅에선 다루지 않으려고 합니다.
* 다만 앞으로 나올 예제에서 새로운 `response`처리방법이 나오면 그때 그때 설명하려고 합니다.

---

## 4. BeautifulSoup

```python
from bs4 import BeautifulSoup
```

* `BeautifulSoup`는 `requests`에 의해 불러들어진 내용들을 탐색하는 라이브러리입니다.
* 이렇게 문자열로 된 `html`데이터에서 필요한 값을 찾아 내는 것을 `파싱`이라고 부릅니다.
* 또한 `BeautifulSoup`는 굳이 웹페이지 상의 `response`가 아니어도 `파싱`이 가능합니다.

```python
html = '''
<html>
  <head>
    <title>BeautifulSoup test</title>
  </head>
  <body>
    <div id='upper' class='test' custom='good'>
      <h3 title='Good Content Title'>Contents Title</h3>
      <p>Test contents</p>
    </div>
    <div id='lower' class='test' custom='nice'>
      <p>Test Test Test 1</p>
      <p>Test Test Test 2</p>
      <p>Test Test Test 3</p>
    </div>
  </body>
</html>'''
```

* 이와 같이 직접 작성한 `html`구문 또한 `BeautifulSopu`로 `파싱`할 수 있습니다.
* `BeautifulSoup`에는 다양한 메소드가 존재하는데 중점적으로 쓰는 몇가지 메소드를 알아보며 사용법을 익혀보고자 합니다.

### 4.1 find 함수

```python
soup = BeautifulSoup(html)

# <h3> 태그를 찾습니다.
soup.find('h3')

# <p> 태그를 찾습니다.
soup.find('p')

# 같은 태그가 여러개 있을 경우 가장 위에 있는 태그만을 출력합니다.
# 또한 한 태그안에 속해있는 속성을 지정해 줄 수 있습니다.

# <div> 태그 중 class 값이 'test'인 <div>태그를 찾습니다.
soup.find('div', class_='test')

# 여러가지의 속성또한 지정해 줄 수 있습니다.
# <div> 태그 중 class 값이 'test'이며 id 값이 'lower'인 <div>태그를 출력합니다.
soup.find('div', class_='test', id='lower')
```

### 4.2 find_all 함수

* `find` 함수가 가장 처음 만나는 태그만을 불러왔다면 `find_all`함수는 조건에 맞는 모든 태그를 불러옵니다.

### 4.3 get_text 함수

```
tag = soup.find('p')
print(tag)
tag.get_text()
```

* 태그 전체를 불러오는 것이 아닌 태그 속 내용만 불러오는 함수 입니다.

### 4.4 CSS를 이용한 tag 찾기

* `find`가 아닌 `select`혹은 `select_one`함수를 사용한다면 `CSS`를 이용하여 태그를 찾을 수 있습니다.

```python
url = 'https://news.v.daum.net/v/20190728165812603'
resp = requests.get(url)

soup = BeautifulSoup(resp.text)
soup.select('h3')

# class 값이 tit_view인 태그들을 가져옵니다.
soup.select('h3.tit_view')
soup.select('.tit_view')

# 이런식의 문법또한 가능합니다.
soup.select('h3[class="tit_view"]')
```

---

## 5. selenium

```python
# import library
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

from bs4 import BeautifulSoup

import time
```

* `selenumn`은 원래 웹 페이지 자동화 모듈 입니다.
* 웹 브라우저를 실제로 실행해 유저가 실제 사용하는 것과 같은 동작이 가능합니다.

```python
# driver라는 변수에 크롬 브라우저를 실행시키는 동작을 할당합니다.
chrome_driver = '/Users/aaron/chromedriver'
driver = webdriver.Chrome(chrome_driver)

# 크롬 브라우저를 이용해 get방식으로 다음과 같은 주소에 request합니다.
# 실제로 크롬 브라우저가 실행되며 아래 주소의 페이지가 실행되는 것을 확인 할 수 있습니다.
driver.get('https://www.python.org/')

# 응답받은 페이지의 정보 내에서 id값이 다음과 같은 요소를 찾아 search에 할당합니다. 이 요소는 주소창입니다
search = driver.find_element_by_id('id-search-field')

# 페이지가 응답할 시간을 잠시 기다린 후 주소창에 만약 무언가 있다면 지웁니다.
time.sleep(2)
search.clear()

# 다시한번 페이지가 응답할 시간을 잠시 기다린후 주소창에 "lambda"라고 작성합니다.
time.sleep(2)
search.send_keys("lambda")

# 다시한번 페이지가 응답할 시간을 잠시 기다린후 키값을 보냅니다. ENTER를 입력한 것과 같습니다.
time.sleep(2)
search.send_keys(Keys.RETURN)

time.sleep(2)

# 브라우저를 종료합니다.
driver.close()
```
* 실제로 `selenum`을 사용하다보면 데이터가 차마 로드 되기 이전에 브라우저가 종료되어 데이터를 저장하지 못하는 경우가 발생합니다.
* 위의 예시 코드는 `time.sleep(2)`라는 구문을 넣어 2초간 기다리게 하였지만 몇초가 걸릴지 모르는 작업을 처리하기에는 부적절 합니다.
* 그렇기에 사용하는 객체가 `WebDriverWait`입니다.
* WebDriverWait(driver, 시간(초)).until(EC.presence_of_element_located((By.CSS_SELECTOR, 'CSS_RULE')))와 같이 사용합니다.

```python
# 다양한 옵션이 있으나 간략하게 설명하자면 `u_cbox_count`라는 데이터가 로드 될 때 까지 최대 10초 기다린다는 의미입니다.
WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.CSS_SELECTOR, '.u_cbox_count')))
```

---
# 마치며
이번 포스팅에서는 `Web Crawing`에 대해 알아보았습니다. `Web Crawing`을 하는 이유는 항상 데이터가 데이터셋으로 존재하는 것은 아니기 때문입니다. 데이터가 존재하지 않는다면 웹 상에서 데이터를 직접 발굴해 이렇게 데이터화 시키는 방법이 무척 유용합니다. 이번 포스팅에서 소개한 라이브러리의 사용방법은 직접 사용하며 여러가지 옵션을 확인해 보는 것이 유용합니다. 그렇기에 조만간 실제 `Web Crawing`을 이용한 실전 프로젝트에 관해서 포스팅 하고자 합니다.