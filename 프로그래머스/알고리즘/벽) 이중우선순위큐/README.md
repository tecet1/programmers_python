## 문제링크
[이중우선순위큐](https://school.programmers.co.kr/learn/courses/30/lessons/42628)

## 첫번째 시도
```
import heapq
from collections import Counter

def solution(operations):
    min_heap = []
    max_heap = []
    
    valid_counts = Counter()
    
    total_size = 0

    for operation in operations:
        operator, operand = operation.split()
        data = int(operand)
        
        if operator == 'I':
            heapq.heappush(min_heap, data)
            heapq.heappush(max_heap, -data)
            
            valid_counts[data] += 1
            total_size += 1
            
        elif operator == 'D':
            if total_size == 0:
                continue

            if data == 1:
                target_heap = max_heap
            else: # data == -1
                target_heap = min_heap

            while target_heap:
                value = -target_heap[0] if target_heap == max_heap else target_heap[0]
                
                if valid_counts[value] > 0:
                    heapq.heappop(target_heap)
                    
                    valid_counts[value] -= 1
                    total_size -= 1
                    break
                else:
                    heapq.heappop(target_heap)
    
    if total_size == 0:
        return [0, 0]
    else:
        while max_heap:
            max_val = -max_heap[0] 
            if valid_counts[max_val] > 0:
                break
            heapq.heappop(max_heap)
            
        while min_heap:
            min_val = min_heap[0]
            if valid_counts[min_val] > 0:
                break
            heapq.heappop(min_heap)
            
        return [max_val, min_val]
```
핵심 아이디어는 두개의 힙을 사용하는 것과, 지연 삭제(lazy delation)개념을 적용하는 것이다.
AI가 알려준 방법인데, 간단하면서도 매우 효과적인 방법이라 알아두면 좋을 것 같다.

## 분석
### 로직
#### 지연 삭제
내가 이해한 지연 삭제는 다음과 같다.
1. 어떤 요소를 지울 때, 실제로 지우는 게 아니라 지워졌다는 표시만 해둔다.
2. 나중에 표시를 참조하여 한번에 요소들을 지운다.

코드에서는 두개의 동일한 자료를 가지는 힙이 있을때,
한쪽에서 삭제가 발생한 경우 동시성을 유지하기 위해 value_count 라는 카운터를 하나 만들어서
해당 카운터에 삭제한 값의 현재 개수가 0보다 크다면 나중에 다른 쪽에서 참조하여 마찬가지로 삭제하는 형태로 지연 삭제가 적용되었다.

약간 운영체제 시간에 배웠던 mutex나 semaphore의 느낌이 나는 것 같다.
어쨌든 카운터+힙 2개를 활용해 흥미로운 풀이를 할 수 있는 방법을 배웠다.

### 문법
#### heapq로 최대힙 구성하기
```
heapq.heappush(max_heap, -data)
```
> Written with [StackEdit](https://stackedit.io/).
