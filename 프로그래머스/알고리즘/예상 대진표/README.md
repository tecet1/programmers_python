
## 문제링크
[예상 대진표](https://school.programmers.co.kr/learn/courses/30/lessons/12985)
## 첫번째 시도
```
def solution(n,a,b):
    answer = 1
    ta,tb=a-1,b-1
    #2로 나눈 몫이 같으면 그 라운드에서 만남. 해당 라운드까지 포함해서 라운드의 개수를 반환해야함.
    while True:
        if ta//2 == tb//2:
            break
        answer+=1
        ta//=2
        tb//=2
    return answer
```
쉬운 문제. 아이디어만 잘 떠올리면 구현도 쉽게 가능하다.

## 분석
### 로직
2로 나눈 몫으로 그룹핑을 해야하는데, 여기서 주의해야 할 점은 순번이 1부터 시작하므로 원본 번호에서 1을 빼줘야한다는 것이다.

### 문법
해당 사항 없음.   
> Written with [StackEdit](https://stackedit.io/).
