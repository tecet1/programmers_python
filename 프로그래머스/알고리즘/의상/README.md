## 문제링크
[의상](https://school.programmers.co.kr/learn/courses/30/lessons/42578)
## 첫번째 시도
```
from collections import Counter

def solution(clothes):
    
    counter = Counter([cloth[1] for cloth in clothes])
    product = 1
    for cloth, frequency in counter.items():
        product*=frequency+1
    return product - 1
```
아이디어는 각 의상종류의 빈도를 counter로 계산하고,
각 의상을 입을 수 있는 가짓수+1를 모두 곱한 뒤 모두 입지 않는 선택지 1을 빼는 것이다.

처음 제시된 문제의 경우 얼굴 2종, 상의 1종, 하의 1종, 겉옷 1종이 있으므로 입지 않는 경우를 고려한다면 3*2*2*2가지의 조합이 있고, 거기서 모두 입지 않는 경우인 1가지를 빼면 총 23가지의 조합이 나온다.

아이디어를 떠올렸다면 구현은 쉽다.

## 분석
### 로직
해당사항 없음.

### 문법
해당사항 없음.

> Written with [StackEdit](https://stackedit.io/).
