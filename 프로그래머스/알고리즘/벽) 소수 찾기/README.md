
## 문제링크
[소수 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

## 첫번째 시도
핵심은 입력으로 받은 문자열로 만들 수 있는 모든 부분 순열을 완전탐색하는 것이다.
즉 순열을 구현해야 하는데, 잘 모르겠어서 그냥 ai를 돌렸다.
정리하면 순열을 구현하는 방법에는
1. itertools.permutaion
2. dfs

이 두가지 방법이 있다.
파이썬을 사용하는 입장에서는 itertools를 활용하는 것이 함수 호출에 의한 오버헤드가 적기 때문에 시간복잡도 측면에서 dfs를 사용하는 것보다 효율적이라고 한다. 
하지만 다른 언어를 사용하는 경우나, 시험장에서 import에 제한을 두고 있는 경우를 고려한다면 dfs를 사용해 구현하는 방법 역시 잘 숙지해야 할 것이다.

또한, 부분 순열을 만들었다면, 만들어진 순열이 소수인지 판별해야하므로, 따로 소수 판별 알고리즘을 만들 필요가 있다.
여기서 소수 판별의 대표 유형에 대해 짚고 넘어가면,
1. 하나의 정수 N에 대한 소수 판별($\mathbf{O(\sqrt{N})}$)
2. 특정 범위 내의 모든 소수 판별 ($\mathbf{O(N \log \log N)}$)

이 두가지 유형이 있고, 1번의 경우 3부터 N의 제곱근까지 해당 수에 나눠본 뒤 나머지가 0인지를 구해 판별할 수 있고,
2번의 경우 잘 알려진 에라스토테네스의 체를 구현해서 판별할 수 있다.

추가적으로 매우 큰 수의 소수 판별의 경우엔 밀러-라빈 소수판별법을 사용할 수 있고, 시간복잡도는 $O(k \log^3 N)$라고 한다. 하지만 지금 당장 필요하지는 않으므로 넘어간다.

이 문제의 경우는 1번에 해당하므로, isprime 함수를 구현해 사용한다.

## itertools.permutation 활용
```
import math
from itertools import permutations

def is_prime_number(n):
    if n < 2:
        return False
    
    if n == 2:
        return True
    
    if n % 2 == 0:
        return False
    
    limit = math.isqrt(n) #정수 제곱근 (floor(sqrt(n))) 적용
    
    for i in range(3, limit + 1, 2):
        if n % i == 0:
            return False
            
    return True

def solution(numbers):
    
    answer = 0
    n = len(numbers)
    numbers_set = set()
    
    for i in range(1, n+1):
        perm = permutations(numbers,i)
        for p in perm:
            number_string = "".join(p)
            numbers_set.add(int(number_string))
            
    for number in numbers_set:
        if is_prime_number(number):
            answer+=1
    print(numbers_set)
    return answer
```

## dfs 활용
```
import math

def is_prime_number(n):
    if n < 2:
        return False
    
    if n == 2:
        return True
    
    if n % 2 == 0:
        return False
    
    limit = math.isqrt(n) #정수 제곱근 (floor(sqrt(n))) 적용
    
    for i in range(3, limit + 1, 2):
        if n % i == 0:
            return False
            
    return True

def solution(numbers):
    
    answer = 0
    n = len(numbers)
    numbers_set = set()
    
    def dfs(current_permutation, remaining_elements):
        if current_permutation:
            numbers_set.add(int(current_permutation))
            
        if not remaining_elements:
            return
        
        for i in range(len(remaining_elements)):
            element = remaining_elements[i]
            new_remaining = remaining_elements[:i] + remaining_elements[i+1:]
            dfs(current_permutation + element, new_remaining)
    
    dfs("", list(numbers))
        
    for number in numbers_set:
        if is_prime_number(number):
            answer+=1
    print(numbers_set)
    return answer
```

## 분석
### 로직
해당사항없음.

### 문법
#### itertools.permutation
```
from itertools import permutations

perm = permutations(numbers,i)
# permutations는 tuple을 반환함
```
> Written with [StackEdit](https://stackedit.io/).
