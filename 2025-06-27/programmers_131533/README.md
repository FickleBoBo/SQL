# [상품 별 오프라인 매출 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131533)

## 문제 설명

다음은 어느 의류 쇼핑몰에서 판매중인 상품들의 상품 정보를 담은 `PRODUCT` 테이블과 오프라인 상품 판매 정보를 담은 `OFFLINE_SALE` 테이블 입니다. `PRODUCT` 테이블은 아래와 같은 구조로 `PRODUCT_ID`, `PRODUCT_CODE`, `PRICE`는 각각 상품 ID, 상품코드, 판매가를 나타냅니다.

| Column name  | Type       | Nullable |
| ------------ | ---------- | -------- |
| PRODUCT_ID   | INTEGER    | FALSE    |
| PRODUCT_CODE | VARCHAR(8) | FALSE    |
| PRICE        | INTEGER    | FALSE    |

상품 별로 중복되지 않는 8자리 상품코드 값을 가지며, 앞 2자리는 카테고리 코드를 의미합니다.

`OFFLINE_SALE` 테이블은 아래와 같은 구조로 되어있으며 `OFFLINE_SALE_ID`, `PRODUCT_ID`, `SALES_AMOUNT`, `SALES_DATE`는 각각 오프라인 상품 판매 ID, 상품 ID, 판매량, 판매일을 나타냅니다.

| Column name     | Type    | Nullable |
| --------------- | ------- | -------- |
| OFFLINE_SALE_ID | INTEGER | FALSE    |
| PRODUCT_ID      | INTEGER | FALSE    |
| SALES_AMOUNT    | INTEGER | FALSE    |
| SALES_DATE      | DATE    | FALSE    |

동일한 날짜, 상품 ID 조합에 대해서는 하나의 판매 데이터만 존재합니다.

## 문제

`PRODUCT` 테이블과 `OFFLINE_SALE` 테이블에서 상품코드 별 매출액(판매가 \* 판매량) 합계를 출력하는 SQL문을 작성해주세요. 결과는 매출액을 기준으로 내림차순 정렬해주시고 매출액이 같다면 상품코드를 기준으로 오름차순 정렬해주세요.

## 예시

예를 들어 `PRODUCT` 테이블이 다음과 같고

| PRODUCT_ID | PRODUCT_CODE | PRICE |
| ---------- | ------------ | ----- |
| 1          | A1000011     | 15000 |
| 2          | A1000045     | 8000  |
| 3          | C3000002     | 42000 |

`OFFLINE_SALE` 테이블이 다음과 같다면

| OFFLINE_SALE_ID | PRODUCT_ID | SALES_AMOUNT | SALES_DATE |
| --------------- | ---------- | ------------ | ---------- |
| 1               | 1          | 2            | 2022-02-21 |
| 2               | 1          | 2            | 2022-03-02 |
| 3               | 3          | 3            | 2022-05-01 |
| 4               | 2          | 1            | 2022-05-24 |
| 5               | 1          | 2            | 2022-07-14 |
| 6               | 2          | 1            | 2022-09-22 |

각 상품 별 총 판매량과 판매가는 다음과 같습니다.

- `PRODUCT_CODE` 가 `A1000011`인 상품은 총 판매량이 6개, 판매가가 15,000원
- `PRODUCT_CODE` 가 `A1000045`인 상품은 총 판매량이 2개, 판매가가 8,000원
- `PRODUCT_CODE` 가 `C3000002`인 상품은 총 판매량이 3개, 판매가가 42,000원

그러므로 각 상품 별 매출액을 계산하고 정렬하면 결과가 다음과 같이 나와야 합니다.

| PRODUCT_CODE | SALES  |
| ------------ | ------ |
| C3000002     | 126000 |
| A1000011     | 90000  |
| A1000045     | 16000  |

---

## 문제 정보

| 난이도 | Lv. 2 |
| ------ | ----- |
| 정답률 | 87%   |

---

## 풀이

```SQL
SELECT
    P.PRODUCT_CODE,
    SUM(P.PRICE * O.SALES_AMOUNT) AS SALES
FROM PRODUCT AS P
JOIN OFFLINE_SALE AS O
    ON P.PRODUCT_ID = O.PRODUCT_ID
GROUP BY P.PRODUCT_CODE
ORDER BY SALES DESC, P.PRODUCT_CODE;
```

---

## 결과

![결과](./assets/스크린샷%202025-06-27%2020.22.51.png)

---
