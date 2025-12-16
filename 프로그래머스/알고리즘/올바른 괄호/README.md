
## 문제링크
[올바른 괄호](https://school.programmers.co.kr/learn/courses/30/lessons/12909)
## 첫번째 시도
```
def solution(s):
    #광탈
    if s[0] == ')':
        return False
    
    stack = [s[0]]
    
    s_length = len(s)
    for i in range(1, s_length):
        if s[i] == '(':
            stack.append('(')
        else: # s[i] == ')'
            # stack이 빈 경우
            if len(stack) == 0:
                return False
            # 이전 문자가 ')'였던 경우
            if stack[-1] == ')':
                return False
            stack.pop()
    
    # 스택에 문자가 남아있는 경우
    if len(stack) > 0:
        return False
    return True
```
쉬운 문제. 아이디어만 잘 떠올리면 구현도 쉽게 가능하다.

## 분석
### 로직
해당 사항 없음.

### 문법
리스트에서 스택을 쓸 때 처럼 pop사용가능.   
```
stack.pop()
```
> Written with [StackEdit](https://stackedit.io/).
