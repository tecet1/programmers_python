
## 문제링크
[완주하지 못한 선](https://school.programmers.co.kr/learn/courses/30/lessons/42576)
## 첫번째 시도
```
from collections import Counter

def solution(participant, completion):
    completion_list = Counter(participant)
    for c in completion:
        completion_list[c] -= 1
    for name, not_completed in completion_list.items():
        if not_completed == 1:
            return name
```
이 문제는 알고리즘 kit의 해시에 분류되어있는 문제로, 키-값 구조의 딕셔너리나 해시테이블을 연습하기 위한 문제인 것 같다.
처음엔 for문을 돌려서 풀려 했는데 시간초과가 났다.

## 분석
### 로직
해당사항 없음.

### 문법
Counter는 딕셔너리와 비슷한 객체를 반환하지만 딕셔너리는 아닌 듯 하다.
보면 딕셔너리를 사용할 때와 같이 dict[a]로 값에 접근할 수 있다.
그리고 .items() 메서드를 통해 for문 이터레이터를 구성할 수 있다.
```
completion_list = Counter(participant)
for c in completion:
        completion_list[c] -= 1
    for name, not_completed in completion_list.items():
        if not_completed == 1:
            return name
```

> Written with [StackEdit](https://stackedit.io/).
