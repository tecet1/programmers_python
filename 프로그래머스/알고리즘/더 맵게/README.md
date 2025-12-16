## 문제링크
[더 맵게](https://school.programmers.co.kr/learn/courses/30/lessons/42626)
## 첫번째 시도
```
import heapq
'''
이번기회에 heapq의 메서드에 익숙해지자
'''

def solution(scoville, K):
    answer = 0
    heapq.heapify(scoville)
    
    while len(scoville) > 1:
        smallest = heapq.heappop(scoville)
        if smallest >= K:
            return answer
        answer+=1
        second_smallest = heapq.heappop(scoville)
        heapq.heappush(scoville, smallest+(2*second_smallest))
        
    smallest = heapq.heappop(scoville)
    if smallest >= K:
        return answer
    return -1
    
```
아이디어는 간단하게 떠올릴 수 있는 문제다.
그런데 종료조건을 잘 세우지 못해서 헤맸다.
가장 중요한건 len(scoville)>1일때 루프를 돌려야 한다는 것이고, 루프를 다 돈 뒤 한번 더 조건 충족 여부를 검사해야 한다는 것도 중요하다.
이 두가지를 간과했다.

## 분석
### 로직
해당 사항 없음.

### 문법
#### heapq의 메서드


**`heapify(x)`**

**리스트를 힙으로 변환**합니다. 기존 리스트를 제자리에서(`in-place`) 최소 힙 구조로 만듭니다.

$O(N)$

**`heappush(heap, item)`**

**요소 삽입**. 힙(`heap`)에 새로운 요소(`item`)를 추가하고 힙 구조를 유지하도록 재정렬합니다.

$O(\log N)$

**`heappop(heap)`**

**가장 작은 요소 추출**. 힙에서 가장 작은 요소(루트)를 제거하고 그 값을 반환합니다.

$O(\log N)$

**`heapreplace(heap, item)`**

**추출 후 삽입**. 가장 작은 요소를 제거하고, 새로운 요소(`item`)를 삽입하는 작업을 한 번에 처리합니다.(`heappop` 후 `heappush`보다 약간 더 효율적일 수 있습니다.)

$O(\log N)$

**`heappushpop(heap, item)`**

**삽입 후 추출**. 새로운 요소(`item`)를 힙에 넣은 다음, 가장 작은 요소를 제거하고 반환합니다. (새로 넣은 요소가 가장 작다면 그것이 바로 추출될 수 있습니다.)

$O(\log N)$

**`nlargest(n, iterable)`**

**가장 큰 `n`개**의 요소를 찾아서 리스트로 반환합니다. 힙을 사용하지만, 최종 결과는 정렬되지 않을 수 있습니다.

$O(N \log k)$

**`nsmallest(n, iterable)`**

**가장 작은 `n`개**의 요소를 찾아서 리스트로 반환합니다.

$O(N \log k)$

> Written with [StackEdit](https://stackedit.io/).
