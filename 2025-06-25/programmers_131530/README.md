# [가격대 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131530)

## 문제 설명

다음은 어느 의류 쇼핑몰에서 판매중인 상품들의 정보를 담은 `PRODUCT` 테이블입니다. `PRODUCT` 테이블은 아래와 같은 구조로 되어있으며, `PRODUCT_ID`, `PRODUCT_CODE`, `PRICE`는 각각 상품 ID, 상품코드, 판매가를 나타냅니다.

| Column name  | Type       | Nullable |
| ------------ | ---------- | -------- |
| PRODUCT_ID   | INTEGER    | FALSE    |
| PRODUCT_CODE | VARCHAR(8) | FALSE    |
| PRICE        | INTEGER    | FALSE    |

상품 별로 중복되지 않는 8자리 상품코드 값을 가지며 앞 2자리는 카테고리 코드를 나타냅니다.

## 문제

`PRODUCT` 테이블에서 만원 단위의 가격대 별로 상품 개수를 출력하는 SQL 문을 작성해주세요. 이때 컬럼명은 각각 컬럼명은 PRICE_GROUP, PRODUCTS로 지정해주시고 가격대 정보는 각 구간의 최소금액(10,000원 이상 ~ 20,000 미만인 구간인 경우 10,000)으로 표시해주세요. 결과는 가격대를 기준으로 오름차순 정렬해주세요.

## 예시

예를 들어 `PRODUCT` 테이블이 다음과 같다면

| PRODUCT_ID | PRODUCT_CODE | PRICE |
| ---------- | ------------ | ----- |
| 1          | A1000011     | 10000 |
| 2          | A1000045     | 9000  |
| 3          | C3000002     | 22000 |
| 4          | C3000006     | 15000 |
| 5          | C3000010     | 30000 |
| 6          | K1000023     | 17000 |

만원 단위의 가격대 별로 상품을 나누면

- 가격대가 0원 ~ 1만원 미만인 상품은 `PRODUCT_ID` 가 2인 상품 1개,
- 가격대가 1만원 이상 ~ 2만원 미만인 상품들은 `PRODUCT_ID` 가 1, 4, 6인 상품 3개,
- 가격대가 2만원 이상 ~ 3만원 미만인 상품은 `PRODUCT_ID` 가 3인 상품 1개,
- 가격대가 3만원 이상 ~ 4만원 미만인 상품은 `PRODUCT_ID` 가 5인 상품 1개,

에 각각 해당하므로 다음과 같이 결과가 나와야 합니다.

| PRICE_GROUP | PRODUCTS |
| ----------- | -------- |
| 0           | 1        |
| 10000       | 3        |
| 20000       | 1        |
| 30000       | 1        |

---

## 문제 정보

| 난이도 | Lv. 1 |
| ------ | ----- |
| 정답률 | 84%   |

---

## 풀이

```SQL
SELECT
    FLOOR(PRICE / 10000) * 10000 AS PRICE_GROUP,
    COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP;
```

---

## 결과

![결과](./assets/스크린샷%202025-06-25%2020.39.21.png)

---
