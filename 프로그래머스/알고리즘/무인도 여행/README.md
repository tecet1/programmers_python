## 문제링크
[무인도 여행](https://school.programmers.co.kr/learn/courses/30/lessons/154540)
## 첫번째 시도

```
import queue

def solution(maps):
    x = [1,1,0,0]
    y = [1,0,1,0]
    rows = len(maps)
    columns = len(maps[0])
    visited = [[False for _ in range(columns)] for _ in range(rows)]
    Q = queue.Queue()
    answer = []
    for i in rows:
        for j in columns:
            tsum = 0
            if not visited[i][j]:
                visited[i][j] = true
                Q.put((i,j))
                
                while not Q.empty():
                    cy,cx=q.get()
                    tsum += map[cy][cx]
                    for k in range(4):
                        tx = cx+x[k]
                        ty = cy+y[k]
                    if tx>=0 and tx<columns and ty>=0 and ty<rows and visited[ty][tx] == false:
                        visited[ty][tx] = true
                        Q.put((tx,ty))
                
                answer.append(tsum)

    sort(answer)
    if len(answer)<=0: return -1
    return answer
```
## **🚫문제점**

**1**

**Iteration**

`for i in rows:` and `for j in columns:`

`rows` and `columns` are **integers** holding the dimensions. Python loops must use `range()` for indexing: `for r in **range(rows):**` and `for c in **range(columns):**`.

**2**

**Boolean Values**

`true` and `false` (lowercase)

Python's boolean literals are **`True`** and **`False`** (capitalized). Using lowercase will result in a `NameError`.

**3**

**Variable Name**

`cy,cx=q.get()` and `tsum += map[cy][cx]`

The queue was initialized as **`Q`** (uppercase), but referenced as `q` (lowercase). The input grid variable is **`maps`**, not `map` (a built-in function).

**4**

**BFS Direction**

`x = [1,1,0,0]`, `y = [1,0,1,0]`

These directions are incorrect for standard 4-way movement (cardinal directions). Standard BFS requires checking **(Up, Down, Left, Right)**: `dx = [0, 0, 1, -1]`, `dy = [1, -1, 0, 0]`.

**5**

**Loop Scope**

The boundary/visited check is **outside** the inner `for k in range(4)` loop.

The check must be **indented inside** the `for k` loop to check _each_ neighbor's coordinates (`tx`, `ty`) as they are generated.

**6**

**Queue Ordering**

Mixing up `(y, x)` and `(row, column)`: `cy,cx=q.get()` then `Q.put((tx,ty))`.

Be consistent. If you retrieve `(row, col)` (or `cy, cx` if you stick to that name), ensure you put `(nr, nc)` (or `tx, ty`) in the same order. Since you are using `rows`/`columns`, the standard convention is `(row, column)`.

**7**

**Sorting**

`sort(answer)` (Standalone function call)

`sort()` is a **method of the list object**. The correct syntax is **`answer.sort()`**.

**8**

**Return Value**

`if len(answer)<=0: return -1`

While your logic might be correct per the requirement, it's safer and cleaner to return a list type. If no regions are found, return `[-1]` as a list if the problem implies returning a list of integers.


## 💡수정된 코드

```
import queue

def solution(maps):
    rows = len(maps)
    columns = len(maps[0])
    
    dy = [1, -1, 0, 0]
    dx = [0, 0, 1, -1]
    
    visited = [[False for _ in range(columns)] for _ in range(rows)]
    Q = queue.Queue()
    answer = []

    for r in range(rows):
        for c in range(columns):
            
            if not visited[r][c] and maps[r][c] != 'X':
                visited[r][c] = True
                Q.put((r, c))
                tsum = 0
                
                while not Q.empty():
                    cr, cc = Q.get()
                    tsum += int(maps[cr][cc])

                    for k in range(4):
                        nr, nc = cr + dy[k], cc + dx[k]

                        if (0 <= nr < rows and 
                            0 <= nc < columns and 
                            not visited[nr][nc] and 
                            maps[nr][nc] != 'X'):
                            
                            visited[nr][nc] = True
                            Q.put((nr, nc))
                            
                answer.append(tsum)

    answer.sort()

    return answer if answer else [-1]
```

