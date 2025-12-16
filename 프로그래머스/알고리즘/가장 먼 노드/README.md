## 문제링크
[가장 먼 노드](https://school.programmers.co.kr/learn/courses/30/lessons/49189)
## 첫번째 시도
인접리스트를 사용하는 문제다.
처음엔 최단거리라는 키워드 때문에 다익스트라를 구현해야하는 어려운 문제인가? 싶었는데 bfs선에서 해결 가능한 문제였다. 

```
import math
import queue

def solution(n, edge):
    
    #인접리스트 초기화
    adjacency_list = [[] for _ in range(n+1)] # 0번 리스트는 미사용
    for u, v in edge:
        adjacency_list[u].append(v)
        adjacency_list[v].append(u)
    
    distance = [math.inf for _ in range(n+1)]
    visited = [False for _ in range(n+1)]
    
    bfs = queue.Queue()
    visited[1] = True
    distance[1] = 0
    bfs.put(1)
    
    while not bfs.empty():
        current_node = bfs.get()
        
        for adjacent_node in adjacency_list[current_node]:
            if not visited[adjacent_node]:
                visited[adjacent_node] = True
                distance[adjacent_node] = distance[current_node]+1
                bfs.put(adjacent_node)
    
    max_distance = max(distance)
    answer = 0
    for i in distance:
        if i == max_distance:
            answer += 1
    return answer
```
----------

## ⚠️ 예외 및 추가 고려 사항

위 코드를 제출하면 예제에서 컷이 나는데 그 이유는 distance 배열에서 0번째 원소에 최대 거리가 할당되어 있기 때문이다. 따라서 이를 적절히 조치하면 해결된다.
```
distance = [math.inf for _ in range(n+1)]
    distance[0] = 0 # 0번째에 할당된 최대값을 0으로 수
    visited = [False for _ in range(n+1)]
```

## 💡수정된 코드
```
import math
import queue

def solution(n, edge):
    
    #인접리스트 초기화
    adjacency_list = [[] for _ in range(n+1)] # 0번 리스트는 미사용
    for u, v in edge:
        adjacency_list[u].append(v)
        adjacency_list[v].append(u)
    
    distance = [math.inf for _ in range(n+1)]
    distance[0] = 0
    visited = [False for _ in range(n+1)]
    
    bfs = queue.Queue()
    visited[1] = True
    distance[1] = 0
    bfs.put(1)
    
    while not bfs.empty():
        current_node = bfs.get()
        
        for adjacent_node in adjacency_list[current_node]:
            if not visited[adjacent_node]:
                visited[adjacent_node] = True
                distance[adjacent_node] = distance[current_node]+1
                bfs.put(adjacent_node)
    
    max_distance = max(distance)
    answer = 0
    for i in distance:
        if i == max_distance:
            answer += 1
    return answer
```

## 분석
### 로직
인접 리스트를 처음 사용해봤는데 상당히 간편하고 좋은 방법이라고 생각한다.

### 문법
인접 리스트 구현은 걍 외우자
```
adjacency_list = [[] for _ in range(n+1)] # 0번 리스트는 미사용
    for u, v in edges:
        adjacency_list[u].append(v)
        adjacency_list[v].append(u)
```

최대값이나 최소값은 float('inf)/float('-inf')로 쉽게 만들 수 있고, math import시 math.inf/-math.inf로도 구현 가능하다.
        
> Written with [StackEdit](https://stackedit.io/).
