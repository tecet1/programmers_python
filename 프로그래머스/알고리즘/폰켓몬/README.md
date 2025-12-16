## 문제링크
[폰켓몬](https://school.programmers.co.kr/learn/courses/30/lessons/1845)
## 첫번째 시도
```
from collections import Counter

def solution(nums):
    n = len(nums)
    counter = Counter(nums)
    answer = 0
    species = len(counter)
    answer = min(n//2,species)
    return answer
```
counter를 써서 구현함.

## 분석
### 로직
해당사항 없음.

### 문법
해당사항 없음.

> Written with [StackEdit](https://stackedit.io/).
