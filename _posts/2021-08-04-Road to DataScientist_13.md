---
layout: post
title:  "Road to Datascientist - 10. Database"
date:   2021-08-04
categories: DataScience 
tags: DataScience Database
---
# Database
---

## 1. Database란?

* `Database`는 컴퓨터시스템에 전자적으로 저장된 체계적 데이터의 모음 입니다.
* 간단하게 말하자면 거대한 저장소라고 생각하셔도 됩니다.

### 1.1 DBMS

* DBMS는 `DataBase Management System`의 약자로 다수의 사용자들이 데이터베이스 내의 데이터를 접근 할 수 있도록 해주는 소프트웨어 도구 입니다.
* 이러한 DBMS는 다음과 같은 4가지 특징을 가지고 있습니다.
    1. 실시간 접근성
    2. 계속적인 변화
    3. 동시 공유
    4. 내용에 따른 참조
    
---
    
## 2. Database의 기본기능

1. 조회
2. 삽입
3. 삭제
4. 수정

### 2.1 동시성 제어

* 동시성 제어의 예를 들면, 한 티켓을 A가 예약을 진행하고 있는 동안은 B가 그 예약에 간섭 할 수 없는 것 입니다.

### 2.2 장애 대응기능

* 데이터의 보호와 장애에 대한 방안이 있어야 합니다.

### 2.3 보안기능

* 사용자에게 보여줄 데이터만 보여줘야 합니다.
* 보안에 위배되는 사항은 전부 데이터베이스 내에서만 처리하여야 합니다.

---

## 3. 관계형 데이터베이스

* 데이터베이스에는 크게 4가지 분류가 존재합니다.

    1. 계층형 데이터베이스
    2. 관계형 데이터베이스
    3. 객체지향형 데이터베이스
    4. NOSQL 데이터 데이터베이스
    
* 그 중 제가 다룰 것은 현재 가장 많이 쓰이고 있는 `관계형 데이터베이스` 입니다.
* `관계형 데이터베이스`는 키(keys)와 값(values)간의 관계를 테이블 화 시킨 데이터베이스 입니다.
* `pandas`의 `DataFrame`과 무척 유사한 형태로 데이터를 저장합니다.


### 3.1 MySQL

* `MySQL`은 관계형 데이터베이스를 다루는 DBMS(RDBMS)입니다.
* 저는 `MySQL`을 앞으로 사용할 것이며 연습할 것 입니다.

---

## 4. SQL

* `SQL`은 관계형 데이터베이스 관리 시스템의 데이터를 관리하기 위해 설계된 ***프로그래밍 언어*** 입니다.

![db_1](/img/db_1.PNG)


* SQL은 일반 프로그래밍 언어에 비해 간결합니다.

### 4.1 SELECT문
```sql
SELECT
        Name, Population
 FROM city
 WHRER CountryCode >= 100
;
```
* 특정 테이블에서 특정 values를 가져오는 구문입니다.
* 위의 코드는 city라는 테이블에서 Name과 Population을 가져오는데 CountryCode가 100이상인 값들만 가져온다 라는 구문입니다.

### 4.2 INSERT문

```sql
INSERT
    INTO 고객연락처
    
 VALUES(7,'ID','NAME', 'E-MAIL', 'E-MAIL@email.com')
;
```

* 특정 테이블에 특정 values를 집어넣는 구문입니다.

### 4.3 UPDATA문

```sql
UPDATA 고객연락처
 SET E-MAIL = 'NEW_email@email.com'
 WHERE 순번 = 1
;
```
* 테이블의 데이터를 수정하는 구문입니다.

### 4.4 DELETE문
```sql
DELETE
  FROM 고객연락처
  WHERE 순번 = 1
;
```
* 테이블의 데이터를 삭제하는 구문입니다.

### 4.5 서브쿼리

* `쿼리`는 데이터를 조작하기 위한 SQL문을 뜻합니다.
* `서브쿼리`는 메인이 되는 커리 속의 쿼리입니다.

```sql
SELECT
    MAX(LIST_PRICE)
FROM PRODUCTS
;
-- 출력값은 8867.99

SELECT
    PRODUCT_ID
    , PRODUCT_NAME
    , LIST_PRICE
FROM PRODUCTS
 WHERE LIST_PRICE = 8867.99
;
```
* 위의 쿼리는 총 두개의 쿼리로 이루어져 있으며 두번의 실행이 필요합니다.

```sql
SELECT
    PRODUCT_ID
    , PRODUCT_NAME
    , LIST_PRICE
FROM PRODUCTS
 WHERE LIST_PRICE = (
                    SELECT
                        MAX(LIST_PRICE)
                    FROM PRODUCTS
                    )
;
```
* 위의 쿼리는 메인쿼리 안에 또하나의 쿼리, `서브쿼리`를 적용함으로써 한번의 실행으로 같은 결과를 내고 있습니다.
* 이와 같이 쿼리속에 쿼리를 넣어 처리하는 것을 `서브쿼리`라고 합니다.

### 4.6 VIEW

* 파이썬의 `함수`를 생각하면 편합니다.
* `함수`는 코드의 재사용성을 방지하기 위해 사용됩니다. `view`또한 같은 SELECT문을 다시 쓰지 않기 위해 존재합니다.

```sql
SELECT A.*
FROM
(
    SELECT
        NAME
        , CREDIT_LIMIT
    FROM CUSTOMERS
) A
;
```
* 이는 `inline view`라 하여 일종의 `서브쿼리`입니다.
* `서브쿼리`를 이용하여 `A`라는 `VIEW`를 생성하였으며 이 `A`를 이용해 `서브쿼리`의 `SELECT`문을 다시 쓰는 것을 방지합니다.

```sql
SELECT
    C.NAME AS CUSTOMER
    , TO_CHAR(A.ORDER_DATE, 'YYYY') AS YEAR
    , SUM( B.QUANTITY * B.UNIT_PRICE ) SALES_AMOUNT
FROM ORDERS A
    , ORDER_ITEMS B
    , CUSTOMERS C
WHERE 1=1
    AND A.STATUS = 'Shipped'
    AND A.ORDER_ID = B.ORDER_ID
    AND A.CUSTOMER_ID = C.CUSTOMER_ID
    GROUP BY C.NAME, TO_CHAR(A.ORDER_DATE, 'YYYY')
    ORDER BY C.NAME
;
```
* 위의 쿼리문과 같이 복수의 `VIEW`를 설정하여 불러 오는 것 또한 가능합니다.

---
# 마치며
이번 포스팅에서는 데이터베이스에 대하여 알아보았습니다. 데이터베이스는 이 외에도 더욱 많은 이론과 개념이 존재합니다만 이번 포스팅에서는 쿼리문을 작성하고 사용하기 위한 내용을 위주로 작성하였습니다. 쿼리문 또한 더 많은 문법이 존재하니 앞으로 실습을 통해 익혀보고자 합니다.