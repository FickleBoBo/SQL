# [헤비 유저가 소유한 장소](https://school.programmers.co.kr/learn/courses/30/lessons/77487)

## 문제 설명

`PLACES` 테이블은 공간 임대 서비스에 등록된 공간의 정보를 담은 테이블입니다. `PLACES` 테이블의 구조는 다음과 같으며 `ID`, `NAME`, `HOST_ID`는 각각 공간의 아이디, 이름, 공간을 소유한 유저의 아이디를 나타냅니다. `ID`는 기본키입니다.

| NAME    | TYPE    |
| ------- | ------- |
| ID      | INT     |
| NAME    | VARCHAR |
| HOST_ID | INT     |

## 문제

이 서비스에서는 공간을 둘 이상 등록한 사람을 "헤비 유저"라고 부릅니다. 헤비 유저가 등록한 공간의 정보를 아이디 순으로 조회하는 SQL문을 작성해주세요.

## 예시

예를 들어, `PLACES` 테이블이 다음과 같다면

| ID       | NAME                                            | HOST_ID  |
| -------- | ----------------------------------------------- | -------- |
| 4431977  | BOUTIQUE STAYS - Somerset Terrace, Pet Friendly | 760849   |
| 5194998  | BOUTIQUE STAYS - Elwood Beaches 3, Pet Friendly | 760849   |
| 16045624 | Urban Jungle in the Heart of Melbourne          | 30900122 |
| 17810814 | Stylish Bayside Retreat with a Luscious Garden  | 760849   |
| 22740286 | FREE PARKING - The Velvet Lux in Melbourne CBD  | 30900122 |
| 22868779 | ★ Fresh Fitzroy Pad with City Views! ★          | 21058208 |

- 760849번 유저는 공간을 3개 등록했으므로 이 유저는 헤비유저입니다.
- 30900122번 유저는 공간을 2개 등록했으므로 이 유저는 헤비유저입니다.
- 21058208번 유저는 공간을 1개 등록했으므로 이 유저는 헤비유저가 아닙니다.

따라서 SQL 문을 실행하면 다음과 같이 나와야 합니다.

| ID       | NAME                                            | HOST_ID  |
| -------- | ----------------------------------------------- | -------- |
| 4431977  | BOUTIQUE STAYS - Somerset Terrace, Pet Friendly | 760849   |
| 5194998  | BOUTIQUE STAYS - Elwood Beaches 3, Pet Friendly | 760849   |
| 16045624 | Urban Jungle in the Heart of Melbourne          | 30900122 |
| 17810814 | Stylish Bayside Retreat with a Luscious Garden  | 760849   |
| 22740286 | FREE PARKING - The Velvet Lux in Melbourne CBD  | 30900122 |

---

## 문제 정보

| 난이도 | Lv. 3 |
| ------ | ----- |
| 정답률 | 74%   |

---

## 풀이

```SQL
SELECT *
FROM PLACES
WHERE HOST_ID IN (
    SELECT HOST_ID
    FROM PLACES
    GROUP BY HOST_ID
    HAVING COUNT(*) >= 2
)
ORDER BY ID;
```

---

## 결과

![결과](./assets/스크린샷%202025-07-04%2010.53.29.png)

---
