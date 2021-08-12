---
layout: post
title:  "Road to Datascientist - 11. SQL 연습 - 1.기후데이터"
date:   2021-08-09
categories: DataScience python Database Oracle
---
# SQL 연습 - 1.기후데이터
---

## 시작하기에 앞서

* 이번 SQL연습에 있어 제가 사용할 DBMS는 `오라클(Oracle)` 입니다.
* `MySQL`과는 문법이나 데이터 타입이 살짝 다릅니다.
* 모든 코드는 아래 링크에 포함되어 있습니다.
> <https://github.com/nanoteyep/Python_practice/tree/master/SQL/TEMPER>

---

## 1. 기후데이터

> <https://data.kma.go.kr/stcs/grnd/grndTaList.do?pgmNo=70>

* 기상자료개방 포털 사이트에서 서울시의 기온 데이터를 다운로드 받을 수 있습니다.
* 지역 구분은 `서울`이며 날자는 `1904-01-01`부터 현재 날자까지 다운로드 받습니다.

* **테이블 생성 및 데이터를 import하는 부분은 생략하겠습니다. 상세는 링크의 코드에 포함되어 있습니다.**

---

## 2. 서울시의 최고/최저 기온 및 해당날자

```sql
SELECT 
        B.STD_DE,
        B.AREA_CD,
        B.MAX_TEMPER,
        B.MIN_TEMPER
    
    FROM
        (
            SELECT
                    SUBSTR(A.STD_DE, 6, 5) AS MMDD,
                    MAX(A.MAX_TEMPER) AS MAX_TEMPER,
                    MIN(A.MIN_TEMPER) AS MIN_TEMPER
                FROM TB_TEMPER_DATA A
                WHERE A.AREA_CD = '108'
                AND A.STD_DE LIKE '_____12-20'
                GROUP BY SUBSTR(A.STD_DE, 6, 5)
            ) A,
            TB_TEMPER_DATA B
    WHERE (B.AREA_CD = '108' AND SUBSTR(B.STD_DE, 6, 5) = A.MMDD  AND B.MAX_TEMPER = A.MAX_TEMPER)
    OR (B.AREA_CD = '108' AND SUBSTR(B.STD_DE, 6, 5) = A.MMDD AND B.MIN_TEMPER = A.MIN_TEMPER);
```

![temper_1](/img/temper_1.png)

---

## 3. 일년중 평균 일교차가 가장 큰 날 구하기

```sql
SELECT
    A.MM,
    A.RANGE
FROM
    (
        SELECT 
            SUBSTR(A.STD_DE, 6, 2) AS MM,
            ROUND(AVG(A.MAX_TEMPER - A.MIN_TEMPER), 2) AS RANGE
        FROM TB_TEMPER_DATA A
        WHERE A.AREA_CD = '108' -- 서울을 나타내는 지역코드, 사실 데이터 자체가 서울시의 온도이기 때문에 별 의미는 없습니다.
        GROUP BY SUBSTR(A.STD_DE, 6, 2)
        ORDER BY RANGE DESC
    ) A
WHERE ROWNUM <= 1; -- 가장 첫 부분만 보여주게 합니다.
```

![temper_2](/img/temper_2.png)

---

## 4. 역사상 가장 일교차가 큰 날자

```sql
SELECT
    A.DAY,
    A.RANGE,
    A.MAX_TEMPER,
    A.MIN_TEMPER

FROM
    (
        SELECT 
            A.STD_DE AS DAY,
            A.MAX_TEMPER - A.MIN_TEMPER AS RANGE,
            A.MAX_TEMPER,
            A.MIN_TEMPER
        
        FROM TB_TEMPER_DATA A
        
        WHERE A.AREA_CD = '108'
        AND A.MAX_TEMPER IS NOT NULL
        AND A.MIN_TEMPER IS NOT NULL
        
        ORDER BY RANGE DESC
    ) A

WHERE ROWNUM <= 1;
```

![temper_3](/img/temper_3.png)

---

## 5. 연도별 평균기온 상승 확인

```sql
SELECT
    SUBSTR(A.STD_DE, 1, 4) AS YYYY,
    ROUND(AVG(AVG_TEMPER), 2) AS AVG_TEMPER

FROM TB_TEMPER_DATA A

GROUP BY SUBSTR(A.STD_DE, 1, 4)
ORDER BY YYYY ASC;

SELECT
    CASE
    WHEN YYYY BETWEEN '1900' AND '1909' THEN '1900년대' WHEN YYYY BETWEEN '1910' AND '1919' THEN '1910년대'
    WHEN YYYY BETWEEN '1920' AND '1929' THEN '1920년대' WHEN YYYY BETWEEN '1930' AND '1939' THEN '1930년대'
    WHEN YYYY BETWEEN '1940' AND '1949' THEN '1940년대' WHEN YYYY BETWEEN '1950' AND '1959' THEN '1950년대'
    WHEN YYYY BETWEEN '1960' AND '1969' THEN '1960년대' WHEN YYYY BETWEEN '1970' AND '1979' THEN '1970년대'
    WHEN YYYY BETWEEN '1980' AND '1989' THEN '1980년대' WHEN YYYY BETWEEN '1990' AND '1999' THEN '1990년대'
    WHEN YYYY BETWEEN '2000' AND '2009' THEN '2000년대' WHEN YYYY BETWEEN '2010' AND '2019' THEN '2010년대'
    WHEN YYYY BETWEEN '2020' AND '2029' THEN '2020년대' END AS AGE,
    ROUND(AVG(AVG_TEMPER), 2) AS AVG_TEMPER
    
FROM
    (
        SELECT
        SUBSTR(A.STD_DE, 1, 4) AS YYYY,
        ROUND(AVG(AVG_TEMPER), 2) AS AVG_TEMPER
        
        FROM TB_TEMPER_DATA A
        
        GROUP BY SUBSTR(A.STD_DE, 1, 4)
        ORDER BY YYYY
    )
    
GROUP BY
    CASE
    WHEN YYYY BETWEEN '1900' AND '1909' THEN '1900년대' WHEN YYYY BETWEEN '1910' AND '1919' THEN '1910년대'
    WHEN YYYY BETWEEN '1920' AND '1929' THEN '1920년대' WHEN YYYY BETWEEN '1930' AND '1939' THEN '1930년대'
    WHEN YYYY BETWEEN '1940' AND '1949' THEN '1940년대' WHEN YYYY BETWEEN '1950' AND '1959' THEN '1950년대'
    WHEN YYYY BETWEEN '1960' AND '1969' THEN '1960년대' WHEN YYYY BETWEEN '1970' AND '1979' THEN '1970년대'
    WHEN YYYY BETWEEN '1980' AND '1989' THEN '1980년대' WHEN YYYY BETWEEN '1990' AND '1999' THEN '1990년대'
    WHEN YYYY BETWEEN '2000' AND '2009' THEN '2000년대' WHEN YYYY BETWEEN '2010' AND '2019' THEN '2010년대'
    WHEN YYYY BETWEEN '2020' AND '2029' THEN '2020년대' END
ORDER BY AGE;
```

![temper_4](/img/temper_4.png)

---

# 마치며
이번 포스팅 부터는 sql문을 실제로 적용하여 연습하려고 합니다. 직접 쿼리문을 작성하며 되짚어보며 능숙하게 사용할 수 있도록 연습합니다.