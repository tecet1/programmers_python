
## 문제링크
[가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42746)
## 첫번째 시도
```
from functools import cmp_to_key

def compare(x,y):
    if x + y > y + x:
        return -1
    elif x + y < y + x:
        return 1
    else: #case equal 
        return 0

def solution(numbers):
    numbers_to_strings = []
    for number in numbers:
        numbers_to_strings.append(str(number))
    numbers_to_strings.sort(key=cmp_to_key(compare))
    answer = ("").join(numbers_to_strings)
    
    if answer[0] == '0': 
        return "0"
    return answer
```
lexicographical order 순으로 정렬하기 위해 커스텀 키 함수를 만들어야 한다.
관련해서 문법을 몰라 ai한테 물어봤다.

## 분석
### 로직
두 숫자를 문자열로 만들어서 더한 값을 비교해서 자릿수순 정렬을 구현한다.

### 문법
sort, sorted에 key를 명시하는 것으로 key로 사용할 함수를 지정할 수 있다. lambda 문법 포함.
```
from functools import cmp_to_key

numbers_to_strings.sort(key=cmp_to_key(compare))
```
> Written with [StackEdit](https://stackedit.io/).