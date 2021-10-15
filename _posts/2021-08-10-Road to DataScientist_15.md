---
layout: post
title:  "Road to Datascientist - 12. SQL 연습 - 2.인구 데이터"
date:   2021-08-10
categories: DataScience 
tags: DataScience Database Oracle
---
# SQL 연습 - 2.인구데이터
---

## 시작하기에 앞서

* 이번 SQL연습에 있어 제가 사용할 DBMS는 `오라클(Oracle)` 입니다.
* `MySQL`과는 문법이나 데이터 타입이 살짝 다릅니다.
* 모든 코드는 아래 링크에 포함되어 있습니다.
> <https://github.com/nanoteyep/Python_practice/tree/master/SQL/POPLTN>

---

## 1. 인구데이터

> <https://jumin.mois.go.kr/>

* 이번 분석에서 다룰 데이터는 행정안전부의 주민등록 인구통계 입니다.
* 전국 읍면동의 2020년 10월 데이터를 사용합니다.

* **테이블 생성 및 데이터를 import하는 부분은 생략하겠습니다. 상세는 링크의 코드에 포함되어 있습니다.**

---

## 2. 전체 인구의 연령대별 비율

```sql
SELECT
    A.AGRDE_SE_CD,
    A.AGRDE_POPLTN_CNT,                                                                             -- 연령대별인구수/총 인구수 를 계산하여 연령대별 인구비율을 구하였습니다.
    TO_CHAR(ROUND((A.AGRDE_POPLTN_CNT/A.SUM_AGRDE_POPLTN_CNT) * 100, 2)) ||'%' AS "POPLTN RATIO",
    A.SUM_AGRDE_POPLTN_CNT AS "TOTAL POPLTN"

FROM
( 
        SELECT
            A.AGRDE_SE_CD,                                                                      -- 각 연령대별 인구수의 합을 구합니다. 즉, 대한민국 총 인구수입니다.
            A.AGRDE_POPLTN_CNT,
            SUM(A.AGRDE_POPLTN_CNT) OVER() AS SUM_AGRDE_POPLTN_CNT
        FROM
        (
            SELECT
                A.AGRDE_SE_CD,
                SUM(A.POPLTN_CNT) AS AGRDE_POPLTN_CNT                                   --연령대 별 인구수 합계입니다.
                
            FROM TB_POPLTN A
            WHERE A.POPLTN_SE_CD = 'T'
            AND A.STD_MT = '202010'
            AND A.ADMINIST_ZONE_NO LIKE '__00000000'
            
            GROUP BY A.AGRDE_SE_CD
            ORDER BY A.AGRDE_SE_CD
        ) A
) A
;
```

![popltn_1](/img/popltn_1.png)

---

## 3. 전체 인구의 남성/여성 비율

```sql
SELECT
    A.POPLTN_SE_CD,
    A.AGRDE_POPLTN_CNT,                                                                 -- 각 성별 별 인구 합을 전체 인구수로 나눠서 성별 비율을 구합니다.
    ROUND(A.AGRDE_POPLTN_CNT/A.TOTAL_AGRDE_POPLTN_CNT * 100, 3) ||'%' AS SEX_RATIO
FROM
(
    SELECT
        A.POPLTN_SE_CD,
        A.AGRDE_POPLTN_CNT,                                                              -- 전체 인구수를 구합니다.
        SUM(A.AGRDE_POPLTN_CNT) OVER() AS TOTAL_AGRDE_POPLTN_CNT
    
    FROM
    (
        SELECT
            A.POPLTN_SE_CD,                                                             -- 성별 별 인구수를 구합니다.
            SUM(A.POPLTN_CNT) AS AGRDE_POPLTN_CNT
        
        FROM TB_POPLTN A
        WHERE A.POPLTN_SE_CD IN ('M', 'F')
        AND A.ADMINIST_ZONE_NO LIKE '__00000000'
        AND A.STD_MT = '202010'
        
        GROUP BY A.POPLTN_SE_CD
        ORDER BY A.POPLTN_SE_CD
    ) A
) A
;
```
![popltn_2](/img/popltn_2.png)

---

## 4. 연령대별 인구비율이 가장 높은 지역

```sql
SELECT
    A.AGRDE_SE_CD,
    A.ADMINIST_ZONE_NO,
    A.ADMINIST_ZONE_NM,
    A.POPLTN_CNT,
    A.POPLTN_RATIO,
    A.RNUM
FROM
(
    SELECT
        A.AGRDE_SE_CD,
        A.ADMINIST_ZONE_NO,
        A.ADMINIST_ZONE_NM,
        A.POPLTN_CNT,
        A.POPLTN_RATIO,
        ROW_NUMBER() OVER(PARTITION BY A.AGRDE_SE_CD ORDER BY A.POPLTN_RATIO DESC) AS RNUM          --RDE_SE_CD를 그룹화 하여 POPLTN_RATIO순으로 내림차순으로 정렬하였으며 ROW_NUMBER를 이용해 순서대로 번호를 RUNM이라는 번호를 매깁니다. AG
    
    FROM
    (
        SELECT
            A.AGRDE_SE_CD,
            A.ADMINIST_ZONE_NO,
            A.ADMINIST_ZONE_NM,
            A.POPLTN_CNT,                                                                           -- RATIO_TO_REPORT를 이용해 ADMINIST_ZONE_NO별 POPLTN_CNT가 차지하는 비율을 구합니다.
            RATIO_TO_REPORT(A.POPLTN_CNT) OVER(PARTITION BY A.ADMINIST_ZONE_NO) AS POPLTN_RATIO
            
        FROM
        (
        SELECT
            A.AGRDE_SE_CD,
            A.ADMINIST_ZONE_NO,
            A.ADMINIST_ZONE_NM,
            A.POPLTN_CNT
            
        FROM TB_POPLTN A
        WHERE A.ADMINIST_ZONE_NO NOT LIKE '_____00000'
        AND A.POPLTN_SE_CD IN ('T')
        AND A.POPLTN_CNT > 0
        AND A.STD_MT = '202010'
        
        ORDER BY ADMINIST_ZONE_NO DESC
        ) A
    ) A
) A
WHERE RNUM <= 1;                            -- RNUM이 1인, 즉 가장 첫번째, 비율이 높은 행만 출력합니다.
```

![popltn_3](/img/popltn_3.png)

---

## 5. 남성보다 여성이 더 많은지역

```sql
SELECT
    A.ADMINIST_ZONE_NM,
    A.FEMALE_POPLTN_CNT - A.MALE_POPLTN_CNT AS OVER_FEMALE              -- 여성인구 - 남성인구를 통해 여성이 얼마나 더 많은지를 구합니다.
FROM
(
    SELECT
        A.ADMINIST_ZONE_NM,
        MAX(A.FEMALE_POPLTN_CNT) AS FEMALE_POPLTN_CNT,                  -- 해당 칼럼에서 여성인구수 만을 출력합니다.
        MAX(A.MALE_POPLTN_CNT) AS MALE_POPLTN_CNT                       -- 해당 칼럼에서 남성 인구수 만을 출력합니다.
    FROM
    (
        SELECT
            A.ADMINIST_ZONE_NM,
            A.POPLTN_SE_CD,
            A.SUM_POPLTN,
            CASE WHEN A.POPLTN_SE_CD = 'F' THEN A.SUM_POPLTN ELSE 0 END AS FEMALE_POPLTN_CNT,       -- POPLTN_SE_CD가 F, 즉 여성이라면 여성의 인구수를 구하고 남성인구를 0으로 둡니다.
            CASE WHEN A.POPLTN_SE_CD = 'M' THEN A.SUM_POPLTN ELSE 0 END AS MALE_POPLTN_CNT          -- POPLTN_SE_CD가 M, 즉 남성이라면 남성의 인구수를 구하고 여성인구를 0으로 둡니다.
        FROM
        (
            SELECT 
                A.ADMINIST_ZONE_NM,
                A.POPLTN_SE_CD,
                SUM(A.POPLTN_CNT) AS SUM_POPLTN                                                     -- 남성과 여성의 인구수 합계를 구합니다.
            FROM TB_POPLTN A 
            WHERE A.ADMINIST_ZONE_NO NOT LIKE '_____00000'
            AND A.POPLTN_SE_CD IN ('F','M')
            
            GROUP BY A.ADMINIST_ZONE_NM, A.POPLTN_SE_CD
        ) A
    ) A
    
    GROUP BY A.ADMINIST_ZONE_NM
) A

ORDER BY OVER_FEMALE DESC;
```

![popltn_4](/img/popltn_4.png)

---

## 6. 남성/여성 비율이 가장 높은지역과 낮은 지역

```sql
SELECT
    *
FROM
(
    SELECT
        A.ADMINIST_ZONE_NM,
        ROUND(A.FEMALE_POPLTN_CNT/A.TOTAL_POPLTN_CNT*100, 2) AS FEMALE_RATIO,
        ROUND(A.MALE_POPLTN_CNT/A.TOTAL_POPLTN_CNT*100, 2) AS MALE_RATIO,
        ROW_NUMBER() OVER(ORDER BY ROUND((FEMALE_POPLTN_CNT/TOTAL_POPLTN_CNT) * 100, 2) DESC) AS FEMALE_RATIO_RANKING,
        ROW_NUMBER() OVER(ORDER BY ROUND((MALE_POPLTN_CNT/TOTAL_POPLTN_CNT) * 100, 2) DESC) AS MALE_RATIO_RANKING
    FROM
    (
        SELECT
            A.ADMINIST_ZONE_NM,
            MAX(A.FEMALE_POPLTN_CNT) AS FEMALE_POPLTN_CNT,
            MAX(A.MALE_POPLTN_CNT) AS MALE_POPLTN_CNT,
            MAX(A.FEMALE_POPLTN_CNT) + MAX(A.MALE_POPLTN_CNT) AS TOTAL_POPLTN_CNT
        FROM
        (
            SELECT
                A.ADMINIST_ZONE_NM,
                CASE WHEN A.POPLTN_SE_CD = 'F' THEN SUM_POPLTN_CNT ELSE 0 END AS FEMALE_POPLTN_CNT,
                CASE WHEN A.POPLTN_SE_CD = 'M' THEN SUM_POPLTN_CNT ELSE 0 END AS MALE_POPLTN_CNT
            FROM
            (
                SELECT 
                    A.ADMINIST_ZONE_NM,
                    A.POPLTN_SE_CD,
                    SUM(A.POPLTN_CNT) AS SUM_POPLTN_CNT
                FROM TB_POPLTN A
                WHERE ADMINIST_ZONE_NO NOT LIKE '_____00000'
                AND POPLTN_SE_CD IN ('F', 'M')
                AND POPLTN_CNT > 0
                
                GROUP BY A.ADMINIST_ZONE_NM, A.POPLTN_SE_CD
            ) A
        ) A
        
        GROUP BY A.ADMINIST_ZONE_NM
    ) A
) A
WHERE A.FEMALE_RATIO_RANKING = 1 OR A.MALE_RATIO_RANKING = 1;
```

![popltn_5](/img/popltn_5.png)

---

# 마치며
이번 포스팅또한 sql연습이었습니다. 좀 더 다양한 데이터를 다루며 쿼리문에 익숙해 집시다.