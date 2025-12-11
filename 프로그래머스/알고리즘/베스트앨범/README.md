
## 문제링크
[베스트앨범](https://school.programmers.co.kr/learn/courses/30/lessons/42579)
## 첫번째 시도
```
def solution(genres, plays):
    '''
    {
    "classic":[classic_total, [(3,800),(0,500),(2,150)]
    "pop":[pop_total, [(4,2500),(1,600)]]
    }
    
    파이썬에 리스트 두개를 각각 key와 value가 되도록 해서 딕셔너리로 만드는 메서드가 있는것으로 기억함
    그걸 쓰면 쉽게 될듯?
    
    '''
    
    # 이 문제에서 index를 추가하는 것과 같이 zip이 할수 있는 일 이상으로 뭔가를 추가해서 딕셔너리를 만드는 경우 그냥 각 리스트를 순회해서 딕셔너리를 만든다.
    album_candidates = {}
    for i in range(len(genres)):
        genre = genres[i]
        play = plays[i]
        if genre not in album_candidates:
            album_candidates[genre] = [play,[(i,play)]] # [total, first_element]
        else:
            album_candidates[genre][0]+=play # add plays to total
            album_candidates[genre][1].append((i,play)) # append play into list
    
    for genre, genre_metadata in album_candidates.items():
        genre_metadata.sort(key = lambda x[1][1]:x, reverse=true) # 장르의 총 재생수 기준으로 내림차순 정렬하는 건데 어케 구현해야할지 잘 모르겠음
        
    album_candidates.sort(key = lambda x.value[0]:x, reverse=true) # 장르의 총 재생수 기준으로 내림차순 정렬하는 건데 어케 구현해야할지 잘 모르겠음
    album = []
    for genre, metadata in album_candidates.items():
        album.append(metadata[0][0])
        album.append(metadata[0][1])
    return album
```
문제자체는 그렇게 어렵지 않은데
구현이 좀 복잡해서 애먹었다.
람다식을 활용한 정렬은 계속 연습해야할 것 같다.

## 문제해결을 위한 단계 (AI 답변)
-   **데이터 구조화:** `zip`을 이용해 (장르, 재생 횟수) 튜플 리스트를 만듭니다. (✅ 완료)
    
-   **정보 집계:** 딕셔너리에 장르별 총 재생 수와 노래 목록을 저장합니다. (✅ 완료)
    
-   **장르 내 노래 정렬:** 각 장르별 노래 목록(`[(인덱스, 재생수), ... ]`)을 **재생 수 내림차순, 인덱스 오름차순**으로 정렬합니다. (⚠️ 수정 필요)
    
-   **장르 순서 정렬:** 전체 장르를 **총 재생 수 내림차순**으로 정렬합니다. (⚠️ 수정 필요)
    
-   **결과 추출:** 정렬된 순서대로 각 장르에서 노래 2개씩을 최종 결과 리스트에 담습니다. (⚠️ 수정 필요)

## 수정된 코드
```
def solution(genres, plays):
    '''
    {
    "classic":[classic_total, [(3,800),(0,500),(2,150)]
    "pop":[pop_total, [(4,2500),(1,600)]]
    }
    
    파이썬에 리스트 두개를 각각 key와 value가 되도록 해서 딕셔너리로 만드는 메서드가 있는것으로 기억함
    그걸 쓰면 쉽게 될듯?
    
    '''
    
    # 이 문제에서 index를 추가하는 것과 같이 zip이 할수 있는 일 이상으로 뭔가를 추가해서 딕셔너리를 만드는 경우 그냥 각 리스트를 순회해서 딕셔너리를 만든다.
    album_candidates = {}
    for i in range(len(genres)):
        genre = genres[i]
        play = plays[i]
        if genre not in album_candidates:
            album_candidates[genre] = [play,[(i,play)]] # [total, first_element]
        else:
            album_candidates[genre][0]+=play # add plays to total
            album_candidates[genre][1].append((i,play)) # append play into list
    
    for genre in album_candidates:
        album_candidates[genre][1].sort(
            key = lambda x: (-x[1], x[0]) #재생 수 기준 내림차순 정렬 후 인덱스 기준 오름차순 정렬
        )
        
    # AI: dict.items()는 (키, 값) 튜플을 반환하며, 정렬 후 다시 딕셔너리로 만들 필요 없이 리스트 상태로 사용합니다.
    #이때 sorted 메서드는 리스트를 반환하는데, 이 메서드를 쓰는 이유는 딕셔너리는 정렬이 불가능하기 때문이다.
    sorted_genres = sorted(
        album_candidates.items(), 
        key=lambda item: item[1][0], 
        reverse=True
    )
        
    album = []
    for genre, metadata in sorted_genres:
        for i in range(min(2,len(metadata[1]))):
            album.append(metadata[1][i][0])
    return album
```

## 분석
### 로직
해당사항 없음.

### 문법
람다식으로 정렬을 위한 key를 제공하는 경우 
```
1.
key = lambda x: (-x[1], x[0]) #재생 수 기준 내림차순 정렬 후 인덱스 기준 오름차순 정렬

2.
key=lambda item: item[1][0]
```
그러니까 key = lambda 매개변수 : 반환할 정렬 기준 값의 구조로 구성할 수 있다는데.. 솔직히 많이 해봐야 익숙해질듯

> Written with [StackEdit](https://stackedit.io/).