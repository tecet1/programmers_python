
## 문제링크
[오프라인/온라인 판매 데이터 통합하기](https://school.programmers.co.kr/learn/courses/30/lessons/131537)
## 첫번째 시도
```
SELECT date_format(SALES_DATE, '%Y-%m-%d') as SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
from ONLINE_SALE 
where sales_date like '2022-03%'

union all 

SELECT date_format(SALES_DATE, '%Y-%m-%d') as SALES_DATE, PRODUCT_ID, null as USER_ID, SALES_AMOUNT
from offline_sale 
where sales_date like '2022-03%'

order by sales_date asc, product_id asc, user_id asc
```

union all에 대해 알게 되었다.
join은 두 테이블이 한 테이블의 pk로 하여금 연관관계를 가질 때, 그 pk를 통해 '접합'하는 개념이고,
union all은 두 테이블이 같은 칼럼들을 가질때, (칼럼이 부족한 경우 null로 대체 가능함) 한 테이블 아래에 다른 테이블의 데이터를 이어붙이는 개념이다.
덧붙여 union은 합집합을 만드는 것으로, union all과의 차이점은 중복값을 지우며 연산시간이 상대적으로 더 느리다는 것이다.

## 문제점
해당 사항 없음.

## 수정된 코드
```
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
FROM ONLINE_SALE 
WHERE SALES_DATE LIKE '2022-03%'

UNION ALL 

SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE, PRODUCT_ID, NULL AS USER_ID, SALES_AMOUNT
FROM OFFLINE_SALE 
WHERE SALES_DATE LIKE '2022-03%'

ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, USER_ID ASC;
```
기존의 코드에서 수정된 것은 null값에 alias(별명)을 붙여 user_id를 null로 대체했음을 명시한 점이다. 

또한, SALES_DATE LIKE '2022-03%'를 WHERE SALES_DATE >= '2022-03-01' AND SALES_DATE < '2022-04-01'로 바꾸는 것이 필터링 성능이 더 좋다고 한다.

## 분석
### 로직
해당사항 없음.

### 문법
#### union all
이어붙이기
### 참조 링크


