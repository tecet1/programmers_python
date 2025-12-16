
## 문제링크
[오픈채팅방](https://school.programmers.co.kr/learn/courses/30/lessons/42888)
## 첫번째 시도

```
def string_maker(argument,nickname):
    str = ""
    if argument == 'Enter':
        #str = "%nickname%님이 들어왔습니다" -> fstring 사용
        str = f"{nickname}님이 들어왔습니다"
    if argument == 'Leave':
        #str = "%nickname%님이 나갔습니다" -> fstring 사용
        str = f"{nickname}님이 나갔습니다"
    return str

def nickname_changer(record,uid,nickname):
    for rec_iterator in record:
        if rec_iterator[1] == uid:
            rec_iterator[3] = string_maker(rec_iterator[0],nickname)

def solution(record):
    answer = []
    #argument, uid, nickname, str를 저장하는 배열로 만들고, 리턴할 때는 str만 리턴하도록 할거임
    for rec in record:
        str = ""
        argument, uid, nickname = rec.split(" ")
        if argument == 'Enter' or argument == 'Leave':
            str = string_maker(argument,nickname)
            answer.append([argument,uid,nickname,str]) 
        elif argument == 'Change':
            #현 시점 answer배열에서 uid가 일치하는 모든 str을 수정?
            #nickname을 수정하도록 하는게 낫지 않을까? -> 그럼 str생성 로직이나 answer저장 로직을 변경해야함
            #str을 다시 생성하도록 하자
            nickname_changer(record,uid,nickname)
    return [sol for sol in answer[3]]
```
## **🚫문제점**

**1a**
**`nickname_changer` 내부 오류**

`nickname_changer` 함수 내부에서 `rec_iterator[3] = string_maker(...)`를 통해 `str`만 수정하고 있습니다. 하지만 `Change` 명령이 발생했을 때 **이전에 저장된 닉네임(`rec_iterator[2]`)도 새 닉네임으로 업데이트**해야 합니다.

`rec_iterator[2] = nickname`을 추가하여 닉네임 정보 자체를 업데이트한 후, 새로운 메시지를 생성하도록 합니다.

**1b**

**문법 오류 (`nickname_changer`)**

`nickname_changer` 함수는 `record`가 아니라 **처리된 리스트(`answer`)**를 받게 될 것이며, 이 리스트는 `argument, uid, nickname, str` 네 요소를 가진 리스트의 리스트입니다. 함수 내부에서 `record`를 순회하며 `if rec_iterator[1] == uid:` 조건이 성립하면, 해당 메시지의 **닉네임을 새로운 닉네임으로 수정**해야 합니다.

**`rec_iterator[2] = nickname`**을 추가하여 닉네임 필드(인덱스 2)를 업데이트해야 합니다.

**2a**

**`answer[3]` 인덱스 오류**

`solution` 함수가 `return [sol for sol in answer[3]]`로 종료됩니다. `answer`는 리스트의 리스트이고, `answer[3]`는 **`answer`의 네 번째 요소** (즉, 네 번째 메시지 전체)를 의미합니다.

모든 최종 메시지 문자열을 담으려면, `answer`의 각 요소(`sol`)에서 **메시지 문자열(인덱스 3)**만 추출해야 합니다. **`return [sol[3] for sol in answer]`**로 수정해야 합니다.

**3a**

**`Change` 로직의 비효율성**

`Change`가 발생할 때마다 **전체 `answer` 리스트**를 순회하며 닉네임을 변경하고 `str`을 다시 생성하는 것은 비효율적입니다.

**최종 닉네임 매핑 테이블(`{uid: final_nickname}`)을 먼저 완성**하고, 그 후에 `Enter`/`Leave` 이벤트만 순회하며 최종 메시지를 한 번에 생성하는 것이 훨씬 효율적입니다.

## 💡수정된 코드

```
def solution(record):
    # 1. 최종 닉네임 매핑 테이블 생성
    uid_to_nickname = {}
    
    for rec in record:
        parts = rec.split(" ")
        argument = parts[0]
        uid = parts[1]
        
        # 'Enter'나 'Change'는 닉네임을 업데이트합니다.
        if argument == 'Enter' or argument == 'Change':
            nickname = parts[2]
            uid_to_nickname[uid] = nickname

    # 2. 최종 메시지 리스트 생성
    answer = []
    
    for rec in record:
        parts = rec.split(" ")
        argument = parts[0]
        uid = parts[1]
        
        # 'Enter'와 'Leave' 이벤트만 출력합니다.
        if argument == 'Enter':
            final_nickname = uid_to_nickname[uid]
            answer.append(f"{final_nickname}님이 들어왔습니다")
        
        elif argument == 'Leave':
            final_nickname = uid_to_nickname[uid]
            answer.append(f"{final_nickname}님이 나갔습니다")
            
        # 'Change'는 출력 메시지를 만들지 않습니다.

    return answer
```

## 분석
### 로직
모든 change 명령이 수행된 후 마지막으로 수정된 nickname만 사용하면 되므로,
1. uid와 nickname을 가지는 맵 자료구조를 하나 만들고 
2. change명령이 나올 때마다 nickname을 수정하여 맵에 반영한 다음
3. 완성된 맵을 활용해 순회를 돌면 더 간단하게 구현이 가능했다.
### 문법
fstring 문법으로 간단하게 formatted string을 구현 가능하다.
```
s=f"{}"
```
> Written with [StackEdit](https://stackedit.io/).
