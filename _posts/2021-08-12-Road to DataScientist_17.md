---
layout: post
title:  "Road to Datascientist - 14. SQL 연습 - 4.상권정보 데이터"
date:   2021-08-12
categories: DataScience 
tags: DataScience Database Oracle
---
# SQL 연습 - 4. 상권정보 데이터
---

## 시작하기에 앞서

* 이번 SQL연습에 있어 제가 사용할 DBMS는 `오라클(Oracle)` 입니다.
* `MySQL`과는 문법이나 데이터 타입이 살짝 다릅니다.
* 모든 코드는 아래 링크에 포함되어 있습니다.
> <https://github.com/nanoteyep/Python_practice/tree/master/SQL/TRDAR>

---

## 1. 상권정보 데이터

> <https://www.data.go.kr/data/15083033/fileData.do>

* 공공데이터 포털 사이트에서 상권정보 데이터를 다운로드 받을 수 있습니다.
* 지역 구분은 `전국`이며 날자는 `2021-06-30`까지 기록된 데이터를 받았습니다.
* 지역별로 파일이 나누어져 있으나 `Python`을 이용하여 데이터를 하나로 합쳐서 사용하였습니다.

* **테이블 생성 및 데이터를 import하는 부분은 생략하겠습니다. 상세는 링크의 코드에 포함되어 있습니다.**

---

## 2. 서울시 강남구 기준 각 업종별 상가 갯수 구하기

```sql
SELECT
    A.CTPRVN_CD,
    A.CTPRVN_NM,
    A.SIGNGU_CD,
    A.SIGNGU_NM,
    A.TRDAR_INDUTY_LRGE_CL_CD,
    A.TRDAR_INDUTY_LRGE_CL_NM,
    COUNT(*) AS CNT                                         -- 전체 행 수, 즉 상가의 갯수 입니다.

FROM TB_TRDAR A
WHERE SIGNGU_CD = '11680'                               -- 강남의 시군 코드입니다.

GROUP BY A.CTPRVN_CD, A.CTPRVN_NM, A.SIGNGU_CD, A.SIGNGU_NM, A.TRDAR_INDUTY_LRGE_CL_CD, A.TRDAR_INDUTY_LRGE_CL_NM
ORDER BY CNT DESC;
```

![trdar_1](/img/trdar_1.png)

---

## 3. 각 지역별 스타벅스 매장 갯수 구하기

```sql
SELECT *
FROM
( 
    SELECT
        A.CTPRVN_CD,
        A.CTPRVN_NM,
        A.SIGNGU_CD,
        A.SIGNGU_NM,
        A.ADSTRD_CD,
        A.ADSTRD_NM,
        COUNT(*) CNT
    FROM TB_TRDAR A
    WHERE ( A.CMPNM_NM LIKE '%스타벅스%'
            OR UPPER(A.CMPNM_NM) LIKE '%'||UPPER('STAR')||'%'||UPPER('BUKS')||'%')                  -- 상호에 '스타벅스' 라는 말을 포함하거나 'STAR BUKS'라는 단어를 포함한 커피전문점을 찾습니다.
    AND A.TRDAR_INDUTY_SMALL_CL_CD = 'Q12A01'                                                       -- 커피 전문점을 나타내는 코드입니다. 
    
    GROUP BY A.CTPRVN_CD, A.CTPRVN_NM, A.SIGNGU_CD, A.SIGNGU_NM, A.ADSTRD_CD, A.ADSTRD_NM
    ORDER BY CNT DESC
) A
WHERE ROWNUM <= 10;
```

![trdar_2](/img/trdar_2.png)

---

## 4. 각 지역별 인구수 대비 커피전문점 갯수 구하기

```sql
SELECT *
FROM
(
    SELECT
        A.ADMINIST_ZONE_NO,
        A.ADMINIST_ZONE_NM,
        A.POPLTN_CNT,
        A.COFFEESHOP_CNT,
        TRUNC(A.POPLTN_CNT/A.COFFEESHOP_CNT) AS SHOP_PER_POPLTN                     -- 매장 당 인구수를 구합니다.
    FROM
    (
        SELECT
            A.ADMINIST_ZONE_NO,
            A.ADMINIST_ZONE_NM,
            A.POPLTN_CNT AS POPLTN_CNT,
            B.SIGNGU_CD,
            COUNT(*) AS COFFEESHOP_CNT
        FROM
        (
            SELECT
                A.ADMINIST_ZONE_NO,
                A.ADMINIST_ZONE_NM,
                SUM(POPLTN_CNT) AS POPLTN_CNT
            FROM TB_POPLTN A
            WHERE A.POPLTN_SE_CD = 'T'
            AND A.ADMINIST_ZONE_NO LIKE '_____00000'
            
            GROUP BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM
        ) A, 
        TB_TRDAR B                                                                  -- 참조하는 테이블을 두개로 지정합니다.
        WHERE SUBSTR(A.ADMINIST_ZONE_NO, 1, 5) = B.SIGNGU_CD                        -- A, 즉 인구수를 참조하는 테이블에서 구한 지역번호와 B, 즉 상가를 조회하는 테이블에서 시군코드가 일치하는 데이터만 가져옵니다.
        AND B.TRDAR_INDUTY_SMALL_CL_CD = 'Q12A01'
        
        GROUP BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM, A.POPLTN_CNT, B.SIGNGU_CD
        ORDER BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM
    ) A
    
    ORDER BY SHOP_PER_POPLTN DESC
)
WHERE ROWNUM <= 10;
```

![trdar_3](/img/trdar_3.png)

---

## 5. 각 지역별 인구수(00~19세) 대비 학원 수 구하기

```sql
SELECT *
FROM
(  
    SELECT
        A.ADMINIST_ZONE_NO,
        A.ADMINIST_ZONE_NM,
        A.SIGNGU_CD,
        A.POPLTN_CNT,
        A.ACADEMY_CNT,
        TRUNC(A.POPLTN_CNT/A.ACADEMY_CNT) AS ACADEMY_PER_POPLTN
    FROM
    (
        SELECT
            A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM, A.POPLTN_CNT AS POPLTN_CNT,
            B.SIGNGU_CD,
            COUNT(*) AS ACADEMY_CNT
            
        FROM
        (
            SELECT
                A.ADMINIST_ZONE_NO,
                A.ADMINIST_ZONE_NM,
                SUM(POPLTN_CNT) AS POPLTN_CNT
            FROM TB_POPLTN A
            WHERE A.POPLTN_SE_CD = 'T'
            AND A.AGRDE_SE_CD IN ('000','010')                                                  -- 10대만 구합니다
            AND A.ADMINIST_ZONE_NO LIKE '_____00000'
            
            GROUP BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM
        ) A,
        TB_TRDAR B
        WHERE SUBSTR(A.ADMINIST_ZONE_NO, 1, 5) = B.SIGNGU_CD
        AND B.TRDAR_INDUTY_LRGE_CL_CD = 'R'                                                     -- 학원을 나타내는 분류 코드입니다.
        
        GROUP BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM, A.POPLTN_CNT, B.SIGNGU_CD
        ORDER BY A.ADMINIST_ZONE_NO, A.ADMINIST_ZONE_NM
    ) A
    
    ORDER BY ACADEMY_PER_POPLTN DESC
)
WHERE ROWNUM <= 10;
```

![trdar_4](/img/trdar_4.png)

---

# 마치며
이번 포스팅을 마지막으로 SQL문 작성 연습을 마무리 하도록 하겠습니다.