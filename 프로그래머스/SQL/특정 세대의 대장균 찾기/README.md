
## 문제링크
[특정 세대의 대장균 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/301650)
## 첫번째 시도
```
with recursive t as (
    SELECT ID, PARENT_ID, 1 as gen
    from ecoli_data
    where parent_id is null
    
    union all
    
    select e.id, e.parent_id, t.gen+1
    from ecoli_data e 
    inner join t on e.parent_id = t.id
)

select id
from t
where gen = 3
order by id
```

SQL에서도 재귀를 사용할 수 있다는 것을 알았다. 아래에서 자세히 분석해보자.

## 문제점
해당 사항 없음.

## 수정된 코드
해당 사항 없음.

## 분석
### 로직
이번 문제는 재귀를 사용해 Recursive CTE라는 방법으로 풀이했다. 원래는 alter를 써서 새로 열 하나를 만들고, 누적합을 반복해서 구하는 방식으로 풀려 했지만, 뭔가 구현하다보니 기존의 누적합을 적용해야하는 알고리즘과는 조금 다르다는 것을 느꼈고, 스스로 해결 방법을 찾지 못해 AI에 질문했다.
Recursive CTE는 모든 row가 필요한 값을 가질 때까지 자기자신을 반복하며 테이블을 업데이트 하는 방식으로, 핵심은 앵커조건 과 union all에 있다. 

먼저 with recursive 테이블이름 as ()로 재귀 테이블을 정의한다.
```
with recursive t as ()
```

그다음으로 앵커조건 또는 초기상태를 설정한다. 이번 문제의 경우 gen이라고 명명하여 풀이했다.

```
with recursive t as (
    select id, parent_id, 1 as gen
    from ecoli_data
    where parent_id is null
)
```
이후 union all을 통해 재귀적으로 새로운 row들을 순회하며 업데이트하도록 구현한다.

이때, 초기상태를 반영한 테이블과 새로 참조할 테이블의 alias가 잘 맞는지 주의한다.
```
with recursive t as (
    select id, parent_id, 1 as gen
    from ecoli_data
    where parent_id is null
    
    union all 
    
    select e.id, e.parent_id, t.gen+1
    from ecoli_data e
    inner join t on t.id = e.parent_id
)
```
마지막으로 새로운 테이블을 사용해 데이터를 추출한다.

```
select id
from t
where gen = 3
order by id
```
원리를 알고나서 다시 보면 대충 어떤 느낌인지 감이 온다. 

이 문제에서 사용한 것과 같이 재귀 쿼리는 매 단계마다 부모 ID로 자식 노드를 찾아가는 과정을 반복하므로, 컬럼에 인덱스가 없다면 매 루프마다 전체 테이블을 풀 스캔(Full Scan)하게 되어 성능이 기하급수적으로 떨어지므로, 대용량 데이터 환경에서는 인덱스를 설계하거나 재귀 깊이 제한(Max Recursion Depth)을 설정하는 등의 예방 조치가 필요하다.

### 문법
해당사항 없음.

### 참조 링크
해당사항 없음.

### AI 답변

