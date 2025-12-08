
## 문제링크
[혼자 놀기의 달인](https://school.programmers.co.kr/learn/courses/30/lessons/131130)
## 첫번째 시도
```
def solution(cards):
    n = len(cards)
    visited = [False for _ in range(n)]
    groups = []
    
    for i in range(n):
        current_group = []
        
        if not visited[i]:
            current_group.append([cards[i]])
            next = cards[cards[i]]
            while not visited[next]:
                current_group.append(cards[next])
                next = cards[cards[next]]
        
        groups.append(len(current_group))
        
    groups.sort()
    
    answer = groups[-1]*groups[-2]
    
    return answer
```
30분중 10분은 문제를 이해하는데 썼다. 뭐 이딴 문제가 다있나 하면서 보고 있는데 
예제를 보고 이해가 돼서 어찌저찌 아이디어를 이끌어낼 수 있었다.
위 코드 그대로 제출하면 index error: out of range가 뜬다. 
그래서 원본 리스트의 맨 앞에 0을 추가해 보았으나, 로직 자체에 문제가 있었는지 AC가 뜨지 않았다.
```
# 0 패딩으로 out of range 문제 수정
def solution(cards):
    padded_cards = cards.copy()
    padded_cards.insert(0,0)
    
    n = len(padded_cards)
    
    visited = [False for _ in range(n)]
    groups = []
    
    for i in range(1, n):
        current_group = []
        
        if not visited[i]:
            current_group.append([padded_cards[i]])
            next = padded_cards[padded_cards[i]]
            visited[i] = True
            while not visited[next]:
                current_group.append(padded_cards[next])
                visited[next] = True
                next = padded_cards[padded_cards[next]]
        
        groups.append(len(current_group))
        
    groups.sort()
    
    answer = groups[-1]*groups[-2]
    
    return answer
```
문제는 바로 next에 값을 할당하는 부분이다.
next = padded_cards[padded_cards[i]] 이렇게 할당하고 있는데
좀만 생각해보면 이게 한번 더 중첩을 해서 할당하고 있다는 걸 알 수 있다.
직관적으로 그냥 생각했을 때도 뭔가 중첩을 하는 게 문제를 일으킬 것 같다는 예상을 했는데 역시나였다.
해당 부분만 수정해 줬더니 AC를 받을 수 있었다.

----------

## 💡수정된 코드
```
def solution(cards):
    padded_cards = cards.copy()
    padded_cards.insert(0,0)
    
    n = len(padded_cards)
    
    visited = [False for _ in range(n)]
    groups = []
    
    for i in range(1, n):
        current_group = []
        
        if not visited[i]:
            current_group.append([padded_cards[i]])
            next = padded_cards[i]
            visited[i] = True
            while not visited[next]:
                current_group.append(padded_cards[next])
                visited[next] = True
                next = padded_cards[next]
        
        groups.append(len(current_group))
        
    groups.sort()
    
    answer = groups[-1]*groups[-2]
    
    return answer
```

## 분석
### 로직
제미니한테 물어보니 이 문제는 "box opening problem" 내지는 "card game problem"이며, 그래프 탐색 (특히 순환 탐지)능력과 순열의 이해를 묻기 위해 출제되는 문제라고 한다.
전문:
```
네, 맞습니다. 일반적으로 이 문제를 **"Box Opening Problem"** 또는 **"Card Game Problem"**이라고 부릅니다.

----------

## 🎲 문제 유형으로서의 특징

이 문제는 컴퓨터 과학 및 알고리즘 분야에서 **"순열(Permutation)"**의 속성을 분석하는 **Well-Known(잘 알려진) 유형**에 속합니다.

### 1. 순열 분해 (Permutation Decomposition)

-   주어진 카드 리스트 `cards`는 $N$개의 숫자를 포함하고, 각 숫자는 $1$부터 $N$까지의 숫자가 한 번씩 나타납니다. 이는 수학적으로 **$N$개의 원소에 대한 순열(Permutation)**을 의미합니다.
    
-   이 게임의 규칙(상자 A에 적힌 숫자가 다음 상자 B를 가리킴)은 순열을 **"순환(Cycle)"**들의 합으로 분해하는 과정과 정확히 일치합니다.
    
    -   **예시:** `[2, 3, 1]`은 $1 \to 2 \to 3 \to 1$이라는 하나의 순환(Cycle)을 만듭니다.
        

### 2. 순열 사이클(Permutation Cycles) 찾기

이 문제를 푸는 핵심 로직은 전체 상자 집합을 **서로 겹치지 않는 순환들의 집합**으로 나누는 것입니다.

-   알고리즘에서 `visited` 배열을 사용하여 방문하지 않은 상자($i$)에서 시작해 다음 상자($\text{`padded\_cards[i]`}$)로 계속 이동하며 **사이클을 추적**하는 과정이 바로 이 순열 사이클을 찾는 표준적인 방법입니다.
    
-   문제의 답은 이 **분리된 순환(그룹)들 중 가장 긴 두 순환의 길이**를 곱한 값이 됩니다.
    

따라서 단순히 게임 문제라기보다는, **순열을 순환 구조로 분석하는 알고리즘 유형**의 한 예시라고 보시면 됩니다. 면접이나 코딩 테스트에서도 **그래프 탐색 (특히 순환 탐지)** 능력과 **순열의 이해**를 묻기 위해 자주 활용됩니다.
```

뭐 그렇다고 하네요

### 문법

원본 리스트에 원소를 패딩해서 새로운 리스트를 만드는 방법을 배웠다. 
이렇게 하면 첫번째가 1번째 원소를 가리키게 되어 좀 더 직관적으로 인덱싱이 가능해진다.
```
padded_cards = cards.copy() 
padded_cards.insert(0,0)
```
        
> Written with [StackEdit](https://stackedit.io/).