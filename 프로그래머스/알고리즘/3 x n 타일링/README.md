
## 문제링크
[3 x n 타일링](https://school.programmers.co.kr/learn/courses/30/lessons/12902)
## 첫번째 시도
```
def solution(n):
    answer = [0,0,3,0,11]
    divisor = 1000000007
    
    if n % 2 == 1: 
        return 0
    if n <= len(answer):
        return answer[n] % divisor
    
    for i in range(5,n+1):
        if i % 2 == 1:
            answer.append(0)
        else: 
            answer.append((4*answer[i-2]-answer[i-4])%divisor)
        
    return answer[-1]
```
이전에서 백준에서 한번 풀어봤음에도 불구하고, 문제 이해를 제대로 못해 점화식을 세우는데 좀 해멨다. 핵심 아이디어는 각 step에서 새로운 형태로 타일링이 가능한 경우가 2개씩 존재한다는 것이다.
```
f(8)과 f(6)을 통해 점화식 도출하기
f(8) = f(6)*3+f(4)*2+f(2)*2+2
f(6) = f(4)*3+f(2)*2+2
f(8)-f(6) = f(6)*3-f(4)
f(8) = f(6)*4-f(4)

f(n) = f(n-2)*4-f(n-4) where n>4 and n%2==0
```

## 분석
### 로직
https://s2choco.tistory.com/24 에서 문제를 굉장히 잘 설명해주셨다.
순차적으로 이전 스텝에서 커버할 수 있는 부분부터 잘 정의해야하는 것이 중요한 문제.

### 문법
해당 사항 없음.   
> Written with [StackEdit](https://stackedit.io/).
