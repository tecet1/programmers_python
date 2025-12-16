
## 문제링크
[다리를 지나는 트럭](https://school.programmers.co.kr/learn/courses/30/lessons/42583)
## 첫번째 시도
```
def solution(bridge_length, weight, truck_weights):
    '''
    bridge_state = [(truck.weight,distance_left)]
    '''
    time = 0
    bridge_state = [] # max_len = bridge_length
    bridge_current_weight = 0
    while truck_weights or bridge_state:
        time += 1
        
        # 지금 다리에 올라가있는 트럭들의 남은 거리가 0이면 다리를 지난 것으로 판정
        if len(bridge_state) > 0: 
            for truck in bridge_state:
                if truck[1] == 0:
                    bridge_current_weight -= truck[0]
                    bridge_state.remove(truck)
        
        # 트럭 추가
        if (len(bridge_state) < bridge_length) and truck_weights: 
            truck_weight = truck_weights[0]
            if bridge_current_weight + truck_weight <= weight:
                truck_weights.pop(0)
                bridge_current_weight += truck_weight
                bridge_state.append([truck_weight, bridge_length])
        
        # 시간 경과로 인한 각 트럭의 남은 거리 감소
        if len(bridge_state) > 0:
            for truck in bridge_state:
                truck[1] -= 1
                    
        #print(time, bridge_state)
    return time
```
시간경과에 따라 어떤 작업을 먼저 수행해야하는지를 생각하는게 어려웠다.
print로 매 시간 다리의 상태를 출력시켜서 예제와 동일하게 나오도록 한 것이 도움이 됐다.
## 수정된 코드
해당사항 없음.

## 분석
### 로직
이런 문제는 '시뮬레이션'문제에 해당한다. 단위시간에 해야할 작업을 알맞게 구성해서 문제에서 요구한대로 각 step에서 잘 동작하고, 종료시점의 상태에 예제와 동일한지를 보고 구현한 코드의 정확성을 판별할 수 있다. 

..그리고 이런 문제를 잘 풀 수 있게 되려면 결국 또 많이 풀어보는 수밖에 없다.

### 문법
해당사항 없음

> Written with [StackEdit](https://stackedit.io/).
