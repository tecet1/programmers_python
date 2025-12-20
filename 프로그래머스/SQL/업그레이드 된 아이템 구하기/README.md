
## 문제링크
[업그레이드 된 아이템 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/273711)
## 첫번째 시도
1. 셀프 조인 2회
```
select i.ITEM_ID, i.ITEM_NAME, i.RARITY
from ITEM_INFO as i 
join ITEM_TREE as t on i.item_id = t.item_id
join ITEM_INFO as j on t.parent_item_id = j.item_id
where t.parent_item_id is not null and j.rarity = 'rare'
order by i.item_id desc
```

2. 중첩 서브쿼리 활용
```
select i.ITEM_ID, i.ITEM_NAME, i.RARITY
from ITEM_INFO as i 
join ITEM_TREE as t on i.item_id = t.item_id
where t.PARENT_ITEM_ID in (
    select item_id
    from item_info
    where rarity = 'rare'
)
order by i.item_id desc
```

두가지 방법으로 구현이 가능하며 성능 차이에 대해서는 아래 AI답변에서 알 수 있다.

## 문제점
해당 사항 없음.

## 수정된 코드
해당 사항 없음.

## 분석
### 로직
해당사항 없음.

### 문법
해당사항 없음.
### 참조 링크
해당사항 없음.
### AI 답변
Q: 
```
ㅇㅋ 다시 문제 풀이로 돌아가서


select i.ITEM_ID, i.ITEM_NAME, i.RARITY
from ITEM_INFO as i 
join ITEM_TREE as t on i.item_id = t.item_id
join ITEM_INFO as j on t.parent_item_id = j.item_id
where t.parent_item_id is not null and j.rarity = 'rare'
order by i.item_id desc


2. 중첩 서브쿼리 활용

select i.ITEM_ID, i.ITEM_NAME, i.RARITY
from ITEM_INFO as i 
join ITEM_TREE as t on i.item_id = t.item_id
where t.PARENT_ITEM_ID in (
    select item_id
    from item_info
    where rarity = 'rare'
)
order by i.item_id desc


이 두가지 방법의 성능 측면에서의 차이점에 대해 분석해봐
```
A:
```
두 가지 쿼리 모두 결과는 동일하지만, 데이터베이스 내부에서 처리하는 **방식(알고리즘)**이 다릅니다. 결론부터 말씀드리면 **데이터의 양과 인덱스 상황에 따라 승자가 달라질 수 있습니다.**

각 방식의 성능 차이를 3가지 핵심 포인트로 분석해 드릴게요.

----------

### 1. 1번 방식 (Self-Join): "큰 지도를 합쳐서 찾기"

테이블 3개를 먼저 다 이어 붙인 뒤, 조건에 맞는 행을 남기는 방식입니다.

-   **장점:** `ITEM_ID`에 인덱스가 잘 걸려 있다면, DB 엔진이 `Nested Loop Join` 등을 통해 매우 빠르게 매칭을 수행합니다. 또한, 부모 아이템(`j`)의 다른 정보(이름 등)가 추가로 필요해져도 바로 출력할 수 있어 확장이 용이합니다.
    
-   **단점 (성능 저하 요인):** `JOIN`은 기본적으로 곱연산의 성격을 띱니다. `ITEM_INFO` 테이블이 아주 크고 `RARITY = 'RARE'` 조건이 데이터를 많이 걸러내지 못한다면, **메모리에 거대한 조인 결과물**을 먼저 올려야 하므로 부담이 커질 수 있습니다.
    

----------

### 2. 2번 방식 (중첩 서브쿼리): "필터 리스트를 먼저 만들기"

부모가 될 수 있는 `ITEM_ID` 목록을 미리 뽑아놓고 대조하는 방식입니다.

-   **장점:** 서브쿼리(`SELECT ITEM_ID FROM ITEM_INFO WHERE RARITY = 'RARE'`)의 결과가 아주 적다면(예: RARE 아이템이 5개뿐일 때), 메인 쿼리는 단 5개의 숫자와만 비교하면 됩니다. 데이터베이스 옵티마이저는 이 방식을 보통 **'세미 조인(Semi-Join)'**으로 최적화하여 조인보다 가볍게 처리하는 경우가 많습니다.
    
-   **단점 (성능 저하 요인):** 만약 `RARE` 등급 아이템이 수만 개라면, `IN (...)` 안에 들어가는 리스트가 너무 커져서 오히려 조인보다 느려질 수 있습니다.
    

----------

### 3. 결정적 차이: 옵티마이저의 지능

현대적인 DB(MySQL 8.0+, Oracle, PostgreSQL 등)에서는 사실 **두 쿼리의 성능 차이가 거의 없을 가능성이 높습니다.**

-   **Query Transformation:** 똑똑한 DB 엔진은 사용자가 2번(서브쿼리)처럼 써도 내부적으로 "음, 이건 1번(조인)으로 푸는 게 더 빠르겠군!"이라고 판단하면 **알아서 조인 방식으로 변환**해서 실행합니다.
    
-   반대로 1번처럼 써도 서브쿼리 방식이 유리해 보이면 내부적으로 그렇게 바꿉니다.
    

----------

### 📊 요약 비교 분석

**항목**

**1번 (JOIN)**

**2번 (SUBQUERY)**

**성능 (대용량)**

인덱스만 있다면 대체로 안정적

필터링된 데이터가 적을 때 매우 빠름

**가독성**

관계 중심적 (A-B-C 연결)

조건 중심적 (부모가 ~인 것)

**메모리 사용**

조인 버퍼 메모리 사용량이 많을 수 있음

서브쿼리 결과를 임시 저장하는 메모리 사용

**추천 상황**

부모의 컬럼(이름 등)을 같이 출력할 때

단순히 존재 여부만 체크할 때

----------

### 💡 실전 팁: 무엇을 써야 할까요?

1.  **가독성 우선:** 질문자님이 보시기에 "부모가 RARE인 아이템"이라는 **논리가 더 잘 보이는 2번**을 먼저 작성하세요.
    
2.  **성능 확인:** 만약 쿼리가 느리다면 `EXPLAIN` 명령어를 앞에 붙여서 실행해 보세요.
    
    -   `EXPLAIN SELECT ...`
        
    -   이때 `DEPENDENT SUBQUERY`라고 뜨면 2번 방식이 느린 것이니 1번(JOIN)으로 바꾸는 게 좋습니다.
        

**이제 어떤 방식이 본인의 스타일과 더 잘 맞으시나요?** 다음에는 이 성능을 직접 눈으로 확인하는 `EXPLAIN` 읽는 법을 간단히 알아볼까요? 아니면 다른 문제를 더 풀어보시겠어요?
```

