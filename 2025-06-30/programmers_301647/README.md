# [부모의 형질을 모두 가지는 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301647)

## 문제 설명

대장균들은 일정 주기로 분화하며, 분화를 시작한 개체를 부모 개체, 분화가 되어 나온 개체를 자식 개체라고 합니다.
다음은 실험실에서 배양한 대장균들의 정보를 담은 `ECOLI_DATA` 테이블입니다. `ECOLI_DATA` 테이블의 구조는 다음과 같으며, `ID`, `PARENT_ID`, `SIZE_OF_COLONY`, `DIFFERENTIATION_DATE`, `GENOTYPE` 은 각각 대장균 개체의 ID, 부모 개체의 ID, 개체의 크기, 분화되어 나온 날짜, 개체의 형질을 나타냅니다.

| Column name          | Type    | Nullable |
| -------------------- | ------- | -------- |
| ID                   | INTEGER | FALSE    |
| PARENT_ID            | INTEGER | TRUE     |
| SIZE_OF_COLONY       | INTEGER | FALSE    |
| DIFFERENTIATION_DATE | DATE    | FALSE    |
| GENOTYPE             | INTEGER | FALSE    |

최초의 대장균 개체의 `PARENT_ID` 는 NULL 값입니다.

## 문제

부모의 형질을 모두 보유한 대장균의 ID(`ID`), 대장균의 형질(`GENOTYPE`), 부모 대장균의 형질(`PARENT_GENOTYPE`)을 출력하는 SQL 문을 작성해주세요. 이때 결과는 ID에 대해 오름차순 정렬해주세요.

## 예시

예를 들어 `ECOLI_DATA` 테이블이 다음과 같다면

| ID  | PARENT_ID | SIZE_OF_COLONY | DIFFERENTIATION_DATE | GENOTYPE |
| --- | --------- | -------------- | -------------------- | -------- |
| 1   | NULL      | 10             | 2019/01/01           | 1        |
| 2   | 1         | 2              | 2019/01/01           | 1        |
| 3   | 1         | 100            | 2020/01/01           | 3        |
| 4   | 2         | 16             | 2020/01/01           | 2        |
| 5   | 4         | 17             | 2020/01/01           | 8        |
| 6   | 3         | 101            | 2021/01/01           | 5        |
| 7   | 2         | 101            | 2022/01/01           | 5        |
| 8   | 6         | 1              | 2022/01/01           | 13       |

각 대장균 별 형질을 2진수로 나타내면 다음과 같습니다.

ID 1 : 1₍₂₎  
ID 2 : 1₍₂₎  
ID 3 : 11₍₂₎  
ID 4 : 10₍₂₎  
ID 5 : 1000₍₂₎  
ID 6 : 101₍₂₎  
ID 7 : 101₍₂₎  
ID 8 : 1101₍₂₎

각 대장균 별 보유한 형질을 다음과 같습니다.

ID 1 : 1  
ID 2 : 1  
ID 3 : 1, 2  
ID 4 : 2  
ID 5 : 4  
ID 6 : 1, 3  
ID 7 : 1, 3  
ID 8 : 1, 3, 4

각 개체별로 살펴보면 다음과 같습니다.

ID 1 : 최초의 대장균 개체이므로 부모가 없습니다.  
ID 2 : 부모는 ID 1 이며 부모의 형질인 1번 형질을 보유하고 있습니다.  
ID 3 : 부모는 ID 1 이며 부모의 형질인 1번 형질을 보유하고 있습니다.  
ID 4 : 부모는 ID 2 이며 부모의 형질인 1번 형질을 보유하고 있지 않습니다.  
ID 5 : 부모는 ID 4 이며 부모의 형질인 2번 형질을 보유하고 있지 않습니다.  
ID 6 : 부모는 ID 3 이며 부모의 형질 1, 2번 중 2 번 형질을 보유하고 있지 않습니다.  
ID 7 : 부모는 ID 2 이며 부모의 형질인 1번 형질을 보유하고 있습니다.  
ID 8 : 부모는 ID 6 이며 부모의 형질 1, 3번을 모두 보유하고 있습니다.

따라서 부모의 형질을 모두 보유한 개체는 ID 2, ID 3, ID 7, ID 8 이며 결과를 ID 에 대해 오름차순 정렬하면 다음과 같아야 합니다.

| ID  | GENOTYPE | PARENT_GENOTYPE |
| --- | -------- | --------------- |
| 2   | 1        | 1               |
| 3   | 3        | 1               |
| 7   | 5        | 1               |
| 8   | 13       | 5               |

---

## 문제 정보

| 난이도 | Lv. 2 |
| ------ | ----- |
| 정답률 | 49%   |

---

## 풀이

```SQL
SELECT
    E1.ID,
    E1.GENOTYPE AS GENOTYPE,
    E2.GENOTYPE AS PARENT_GENOTYPE
FROM ECOLI_DATA AS E1
JOIN ECOLI_DATA AS E2
    ON E1.PARENT_ID = E2.ID
WHERE E1.GENOTYPE & E2.GENOTYPE = E2.GENOTYPE
ORDER BY E1.ID;
```

---

## 결과

| 채점 결과        |
| ---------------- |
| 테스트 1 〉 통과 |
| 테스트 2 〉 통과 |
| 테스트 3 〉 통과 |
| 테스트 4 〉 통과 |
| 테스트 5 〉 통과 |
| 테스트 6 〉 통과 |

---
