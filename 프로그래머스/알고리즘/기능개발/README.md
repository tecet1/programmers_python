## 문제링크
[기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)
## 첫번째 시도
```
import math

def solution(progresses, speeds):
    '''
    days_to_complete = [ceil((100-progresses[i])/speeds[i]) for i in range(len(speeds))]
    '''
    answer = []
    days_to_complete = [math.ceil((100-progresses[i])/speeds[i]) for i in range(len(speeds))]
    
    while len(days_to_complete)>0:
        prev = days_to_complete[0]
        sum = 0
        while prev >= days_to_complete[0]:
            sum += 1
            days_to_complete.pop(0)
        answer.append(sum)
    print(days_to_complete)
    return answer
```
이대로 제출시 list out of range가 뜨는데, 그 이유는 pop을 수행하는 루프에서 리스트가 비어있을 경우를 고려하지 않았기 때문이다. 따라서 이를 수정해야한다.

## 수정된 코드
```
import math

def solution(progresses, speeds):
    '''
    days_to_complete = [ceil((100-progresses[i])/speeds[i]) for i in range(len(speeds))]
    '''
    answer = []
    days_to_complete = [math.ceil((100-progresses[i])/speeds[i]) for i in range(len(speeds))]
    
    while days_to_complete:  
        prev = days_to_complete[0]
        days_to_complete.pop(0)
        deploy_count = 1
    
        while days_to_complete:
            if prev >= days_to_complete[0]:
                deploy_count += 1
                days_to_complete.pop(0)
            else:
                break
            
        answer.append(deploy_count)
    return answer
```

## 분석
### 로직
큐/스택 자료구조에서 루프를 돌며 요소를 삭제하는 경우, 종료조건에 반드시 자료구조의 크기가 0인 경우를 추가해야 한다.

### 문법
해당사항 없음

> Written with [StackEdit](https://stackedit.io/).