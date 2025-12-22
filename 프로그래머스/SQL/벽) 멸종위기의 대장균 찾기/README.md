
## 문제링크
[멸종위기의 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301651)
## 첫번째 시도
1. left join
```
select count(*) as COUNT, gen as GENERATION
from t
left join (
    select parent_id, count(*) as childs
    from t
    group by parent_id
) sub 
on t.id = sub.parent_id
where sub.childs is null
group by gen
```

2. not exists
```
select 
    count(*) as COUNT,
    gen as GENERATION
from t as parent
where not exists(
    select 1
    from t as child
    where parent.id = child.parent_id
)
group by gen;
```
앞선 문제 [특정 세대의 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301650) 에서 구한 recursive CTE를 사용해 이번에는 조건에 부합하는 row의 개수를 반환해야하는 문제이다.

left join을 통해 풀었고, 이후 AI가 not exists를 사용한 방법을 알려줬다. not exists를 사용하는 경우, 대용량 데이터에 더 잘 대응할 수 있다고 한다. 이에 대한 AI의 설명은 아래에 기재한다.

## 문제점
해당 사항 없음.

## 수정된 코드
해당 사항 없음.

## 분석
### 로직
추가적으로, 해당 세대의 총 개체수와 자식이 없는 개체수를 동시에 세는 경우 아래와 같이 코드를 짤 수 있다. 
```
SELECT 
    gen AS GENERATION,
    COUNT(*) AS TOTAL_COUNT,
    COUNT(CASE WHEN sub.childs IS NULL THEN 1 END) AS NO_CHILD_COUNT
FROM t
LEFT JOIN (
    SELECT parent_id, count(*) as childs
    FROM t
    GROUP BY parent_id
) sub ON t.id = sub.parent_id
GROUP BY gen;
```

위 코드에서 흥미로웠던 것은 count의 사용이다.
인수 안에 \*를 넣어 그저 만족하는 모든 row를 세는 것 뿐만 아니라,
조건에 만족하는 개체를 세도록 인수를 조정할 수 있다는 걸 알았기 때문이다.
count의 용례는 다음과 같다.

```
-   COUNT(*): 행(Row) 자체를 세기 때문에, 특정 컬럼이 NULL이라도 상관없이 모든 행의 개수를 셉니다.
    
-   COUNT(컬럼명): 해당 컬럼의 값이 `NULL`인 행은 빼고 숫자를 셉니다.
    
-   COUNT(CASE ... END): 조건에 맞을 때만 값을 반환하게 하여 조건부 카운트를 가능하게 합니다.
```
위 코드의 경우 마지막 용례를 사용한 것인데, `CASE WHEN sub.childs IS NULL THEN 1 END` 은 `ifnull(sub.childs, 1)` 와 동일한 것이 아닌지 생각했으나, 후자의 경우 모든 row에 대해 null이 아닌 값을 갖게 하므로 원하는 대로 동작하지 않을 것을 예상할 수 있다.

### 문법
해당사항 없음.

### 참조 링크
해당사항 없음.

### AI 답변
```
데이터베이스 관점에서 `NOT EXISTS`가 `LEFT JOIN + IS NULL` 방식보다 성능상 유리한 이유는 크게 두 가지입니다: **불필요한 데이터 결합의 생략**과 **조기 종료(Early Exit)** 전략 때문입니다.

### 1. 조기 종료 (Early Exit) 알고리즘

이것이 `NOT EXISTS`의 가장 큰 장점입니다.

-   **`LEFT JOIN`**: 자식이 1명이든 100명이든 일단 모든 자식 데이터를 가져와서 부모 데이터 옆에 다 붙여봅니다. 그 후 `IS NULL`을 체크하기 위해 전체 결과셋을 뒤져야 합니다.
    
-   **`NOT EXISTS`**: 부모 행 하나를 검사할 때, 자식 테이블에서 해당 `parent_id`를 가진 행을 **딱 하나라도 찾는 순간** 즉시 탐색을 멈춥니다.
    
    -   "자식이 있는지 없는지"만 알면 되기 때문에, 하나라도 있으면 그 부모는 바로 탈락(False) 처리하고 다음 부모로 넘어갑니다. 굳이 자식 100명을 다 세고 있을 필요가 없는 것이죠.
        

----------

### 2. 메모리 및 I/O 효율성

-   **조인 방식**: 두 테이블을 합친 거대한 가상 테이블을 메모리에 생성해야 합니다. 특히 자식 테이블에 데이터가 많을 경우, 이 가상 테이블의 크기가 커져 메모리가 부족해지고 디스크 I/O가 발생할 확률이 높습니다.
    
-   **NOT EXISTS 방식**: 추가적인 가상 테이블을 생성하지 않고 필터링 조건으로만 작동합니다. 따라서 메모리 사용량이 훨씬 적고 경제적입니다.
    

----------

### 3. 실행 계획(Execution Plan)의 차이

데이터베이스 엔진(Optimizer)은 상황에 따라 다르지만 보통 다음과 같은 전략을 취합니다.

**방식**

**주요 전략**

**특징**

**LEFT JOIN**

**Nested Loop** 또는 **Hash Join**

모든 데이터를 매칭하느라 연산량이 많음.

**NOT EXISTS**

**Anti Semi Join**

"매칭되지 않는 것"을 찾는 특수 알고리즘을 사용하여 매우 빠름.

----------

### 4. 주의사항: 인덱스(Index)의 유무

하지만 이 성능 이점을 누리기 위해서는 **반드시 인덱스**가 필요합니다.

-   `child` 테이블의 `parent_id` 컬럼에 인덱스가 걸려 있어야 합니다.
    
-   인덱스가 있다면 `NOT EXISTS`는 인덱스 트리만 살짝 훑고 "어, 자식 있네?" 하고 바로 넘어갈 수 있지만, 인덱스가 없다면 매번 자식 테이블 전체를 풀 스캔(Full Scan)해야 하므로 성능이 급격히 떨어집니다.
    

----------

### 💡 결론

"존재 여부"만 체크하면 되는 지금 같은 문제(자식이 없는 부모 찾기)에서는 **`NOT EXISTS`가 의도도 명확하고 성능상으로도 베스트**입니다.

**혹시 지금 테이블 `t`에 `id`나 `parent_id` 컬럼에 인덱스가 설정되어 있나요? 인덱스 설정 여부를 확인하는 방법을 알려드릴까요?**
```

