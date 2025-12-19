
## 문제링크
[서울에 위치한 식당 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131118)
## 첫번째 시도
```
SELECT i.REST_ID, i.REST_NAME, i.FOOD_TYPE, i.FAVORITES, i.ADDRESS, round(avg(r.REVIEW_SCORE),2) as SCORE
from rest_info as i join rest_review as r on i.rest_id = r.rest_id
where i.address like '서울특별시%'
order by SCORE desc, i.FAVORITES desc
```

## 문제점
avg함수를 사용함에 있어서 
groupby를 지정하지 않아 의도한대로 작동하지 않는다.
실제로, 테스트를 돌려보면 그때그때 다른 값을 반환했다.
따라서, where 하단에 groupby i.rest_id를 삽입해 평균을 어떤 그룹에서 낼 것인지를 명시해야한다.

## 수정된 코드
```
SELECT 
    i.REST_ID, 
    i.REST_NAME, 
    i.FOOD_TYPE, 
    i.FAVORITES, 
    i.ADDRESS, 
    ROUND(AVG(r.REVIEW_SCORE), 2) AS SCORE
FROM REST_INFO AS i 
JOIN REST_REVIEW AS r ON i.REST_ID = r.REST_ID
WHERE i.ADDRESS LIKE '서울%'
GROUP BY i.REST_ID  -- 식당별로 그룹화 필수!
ORDER BY SCORE DESC, i.FAVORITES DESC;
```

## 분석
### 로직
해당사항 없음.

### 문법
sum, avg와 같은 집계 함수 사용시 반드시 group by를 함께 사용한다.

> Written with [StackEdit](https://stackedit.io/).

