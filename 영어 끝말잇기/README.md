
## 문제링크
[영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)
## 첫번째 시도
```
def solution(n, words):
    answer = [0,0]
    # answer = [순번, 순서]
    # 순번은 1번부터 시작하므로 n으로 나눈 나머지임.
    # 순서는 n으로 나눈 몫+1임.
    words_length = len(words)
    word_set = {words[0]}
    previous_word = words[0]
    
    for i in range(1, words_length):
        player_number = (i % n) + 1
        turn_number = (i // n) + 1
        
        #끝말로 시작하지 않는 경우
        if previous_word[-1] != words[i][0]:
            return [player_number, turn_number]
        
        #이미 나온 단어를 말한 경우
        if words[i] in word_set:
            return [player_number, turn_number]
        
        else:
            word_set.add(words[i])
            previous_word = words[i]
    return answer
```
쉬운 문제. 아이디어만 잘 떠올리면 구현도 쉽게 가능하다.

## 분석
### 로직
해당 사항 없음.

### 문법
딕셔너리(혹은 이 경우에선 집합)에 요소를 추가하는 방법
```
word_set.add(words[i])
```   
> Written with [StackEdit](https://stackedit.io/).
