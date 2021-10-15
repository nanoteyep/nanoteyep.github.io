---
layout: post
title:  "Road to Datascientist - 13. SQL 연습 - 3.대중교통 데이터"
date:   2021-08-11
categories: DataScience
tags: DataScience Database Oracle
---
# SQL 연습 - 3.대중교통 데이터
---

## 시작하기에 앞서

* 이번 SQL연습에 있어 제가 사용할 DBMS는 `오라클(Oracle)` 입니다.
* `MySQL`과는 문법이나 데이터 타입이 살짝 다릅니다.
* 모든 코드는 아래 링크에 포함되어 있습니다.
> <https://github.com/nanoteyep/Python_practice/tree/master/SQL/PBTRNSP>

---

## 1. 대중교통 데이터

> <https://pay.tmoney.co.kr/ncs/pct/ugd/ReadTrcrStstList.dev>

* 티머니 홈페이지에서 대중교통 통계자료를 다운로드 받을 수 있습니다.
* 2021년 07월 데이터를 사용합니다.

* **테이블 생성 및 데이터를 import하는 부분은 생략하겠습니다. 상세는 링크의 코드에 포함되어 있습니다.**

---

## 2. 승하차 인원이 가장 많은 역

```sql
SELECT *
FROM
(
    SELECT
        A.STATN_NO,
        A.STATN_NM,
        A.HO_LN,
        A.TK_NMPR_CNT,
        A.GF_NMPR_CNT,
        ROW_NUMBER() OVER(ORDER BY A.TK_NMPR_CNT DESC) AS RANKING_TK_DESC,
        ROW_NUMBER() OVER(ORDER BY A.TK_NMPR_CNT) AS RANKING_TK_ASC,
        ROW_NUMBER() OVER(ORDER BY A.GF_NMPR_CNT DESC) AS RANKING_GF_DESC,
        ROW_NUMBER() OVER(ORDER BY A.GF_NMPR_CNT) AS RANKING_GF_ASC
    FROM
        (
        SELECT
            A.STATN_NO,
            A.STATN_NM,
            A.HO_LN,
            MAX(A.TK_NMPR_CNT) AS TK_NMPR_CNT,
            MAX(A.GF_NMPR_CNT) AS GF_NMPR_CNT
        FROM
        (
            SELECT
                A.STATN_NO,
                A.STATN_NM,
                A.HO_LN,
                CASE WHEN A.TKCAR_GFF_SE_CD = 'TK' THEN A.NMPR_CNT ELSE 0 END AS TK_NMPR_CNT,
                CASE WHEN A.TKCAR_GFF_SE_CD = 'GF' THEN A.NMPR_CNT ELSE 0 END AS GF_NMPR_CNT
            FROM
            ( 
                SELECT
                    A.STATN_NO,
                    A.STATN_NM,
                    A.HO_LN,
                    A.TKCAR_GFF_SE_CD,
                    SUM(NMPR_CNT) AS NMPR_CNT
                    
                FROM TB_PBTRNSP A
                
                GROUP BY A.STATN_NO, A.STATN_NM, A.HO_LN, A.TKCAR_GFF_SE_CD
                ORDER BY NMPR_CNT DESC
            ) A
        ) A
        
        GROUP BY A.STATN_NO, A.STATN_NM, A.HO_LN
    ) A
) A
WHERE A.RANKING_TK_DESC = 1 OR A.RANKING_TK_ASC = 1 OR A.RANKING_GF_DESC = 1 OR A.RANKING_GF_ASC = 1;
```

![pbtrnsp_1](/img/pbtrnsp_1.png)

---

## 3. 수도권 지하철 중 가장 승하차 인원수가 많은 역

```sql
SELECT *
FROM
(
    SELECT
        A.STATN_NO,
        A.STATN_NM,
        A.HO_LN,
        A.NMPR_CNT,
        ROW_NUMBER() OVER(PARTITION BY A.HO_LN ORDER BY A.NMPR_CNT DESC) AS RANKING_NMPR
    FROM
    (
        SELECT
            A.STATN_NO,
            A.STATN_NM,
            A.HO_LN,
            SUM(A.NMPR_CNT) AS NMPR_CNT
        FROM TB_PBTRNSP A
        
        GROUP BY A.STATN_NO, A.STATN_NM, A.HO_LN
    ) A
) A
WHERE RANKING_NMPR = 1

ORDER BY A.NMPR_CNT DESC;
```

![pbtrnsp_2](/img/pbtrnsp_3.png)

---

## 4. 23시 이후에 가장 많이 승차하는 역

```sql
SELECT
    *
FROM
(
    SELECT
        A.STATN_NO,
        A.STATN_NM,
        A.HO_LN,
        A.NMPR_CNT,
        ROW_NUMBER() OVER(ORDER BY A.NMPR_CNT DESC) AS RANKING_NMPR
    FROM
    (
        SELECT
            A.STATN_NO,
            A.STATN_NM,
            A.HO_LN,
            SUM(A.NMPR_CNT) AS NMPR_CNT
        
        FROM TB_PBTRNSP A
        WHERE (A.BEGIN_TIME, A.END_TIME) IN (('2300','2400'),('0000','0100'),('0100','0200'),('0200','0300'),('0300','0400'))       -- 23시부터 04시까지로 시간을 한정합니다.
        AND A.TKCAR_GFF_SE_CD = 'TK'
        
        GROUP BY A.STATN_NO, A.STATN_NM, A.HO_LN
    ) A
) A
WHERE A.RANKING_NMPR <= 10;
```

![pbtrnsp_3](/img/pbtrnsp_3.png)

---

# 마치며
이번 포스팅도 마찬가지로 sql연습이었습니다. sql연습은 다음 포스팅으로 마무리 하려고 합니다.