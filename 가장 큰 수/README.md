
## 문제링크
[H-index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)
## 첫번째 시도
```
def solution(citations):
    '''
    일단 내림차순으로 정렬?
    h는 n보다 클 수 없음, 반복문을 돌린다면 기준은 n이 될 것.
    딕셔너리를 만들어서 citation의 모든 원소의 빈도를 계산하고 {citation: frequency}의 형태로 구성
    lambda x:x.key를 정렬함수의 key함수로 설정
    그럼 이제 h값을 계산해야 되는데.. 
    예제의 경우 6이 맨 위에 있을거니까 6부터 시작한다고 가정하면 일단 n=5에 비해 크므로 바로 컷
    그럼 다음스텝으로 넘어가는데 어떤 값으로 넘어가나? -1한 5? 일단 그렇게 해보자. 그럼 5일때, 5번 이상 인용된 논문은 2개.
    이걸 컴퓨터가 계산하려면 만든 딕셔너리의 key가 현재 h에 비해 큰 경우의 value를 모두 더해야함. 
    그럼 부분합을 쓸까? 그럼 {6:1, 5:2, 3:3, 1:4, 0:5}이런 형태로 딕셔너리를 만들 수 있다. 그럼 간편하긴하다.
    그럼 이제 h를 최대 key에서부터 최소 key까지 순회하다가 True인 경우의 값을 반환하도록 만드는 로직으로 간다고 하면,
    6,5까지는 동일하고 4로 넘어갔을 때 판별을 어떻게 효율적으로 하지? >> 아니다 그냥 key값만 갖고 순회 하면 되는 것 같다.
    그럼 for key in dict로 순회하고 만약 key<=value면 그 key가 곧 정답이다. 
    빈도수 계산은 저번에 counter라는 방법이 있었으니 그걸로 한번 계산해보고 만들어진 딕셔너리를 key값 기준으로 내림차순 정렬
    그리고 for key in dict로 순회하면 될 것 같다
   
    citations.sort(reverse=True)
    H_index = Counter(citations)
    H_index = sorted(H_index.items(), key=lambda item: item[0], reverse=True)
    
    accumulated_papers = 0
    for key,value in H_index:
        accumulated_papers += value
        
        if key <= accumulated_papers:
            return key
        
    틀렸는데 너무 어렵게 생각했나 싶기도 하다.
    그냥 정렬한 다음에 인용 수 큰 값부터 하나씩 내려가면서 누적 합 변수 하나 설정해두고
    순회하면 되는거 아닌가?
       
    citations.sort(reverse=True)
    accumulated_papers=0
    for citation in citations:
        accumulated_papers += 1
        if citation <= accumulated_papers:
            return citation
    
    처음 코드랑 로직이 동일한데 더 간단하게 짜봤지만 로직 자체에 문제가 있는 것 같다.
    아. 위 로직으로 코드가 돌아가면 [44,22]와 같은 입력이 들어오면 22 이하의 값은 탐색자체를 하지 않는다. 하지만 이 경우 h-index는 2가 되어야 한다.
    그럼 결국 순회는 n을 기준으로 0부터 n까지 순회해야 한다는 것을 알 수 있다.
    '''
    citations.sort(reverse=True)
    n = len(citations)
    for i in range(n):
        citation_count = citations[i]
        paper_count = i + 1
        if citation_count < paper_count:
            return i
    return n
```


## 분석
### 로직
세는 기준을 세우는 데서 헤맸다. 이런 사고 훈련도 많이 해보면 해볼 수록 늘 것이라고 생각한다.

### 문법
해당사항없음.

> Written with [StackEdit](https://stackedit.io/).