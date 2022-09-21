---
layout: single
title: "[Algorithm] Stack/Queue and DFS/BFS"
categories: Algorithm
tags: [Algorithm, python]
---


### Stack 은 FILO (First in Last out) 이고, Queue 는 FIFO (First in First out) 이다
- Stack, Queue 모두 \'deque\' 를 사용한다
- Stack 은 list 를 사용해도 시간복잡도에 문제가 없지만, Queue 는 문제가 있다
  - list.pop(0) 은 시간복잡도가 O(n) 이지만, deque.popleft() 는 시간복잡도가 O(1) 이다


```python
from collections import deque

A = [2, 4, 8]

stack = []  # stack = deque()

for i in range(len(A)):
    stack.append(A[i])
for i in range(len(A)):
    print(stack.pop())

queue = deque(); print()

for i in range(len(A)):
    queue.append(A[i])
for i in range(len(A)):
    print(queue.popleft())
```

    8
    4
    2
    
    2
    4
    8
    

### Tree 는 순환이 없는 Graph 이다
그러므로 vertex 가 $n$ 개이면 edge 는 $n-1$ 개이다 <br>
따라서 먼저 vertex 의 개수 $n$ 만 주어지고 <br>
두 번째 줄부터 $n-1$ 개의, ( 부모 vertex, 자식 vertex ) 가 주어진다고 하자 <br>
<img src="/assets/img/Tree.png"> <br>
위의 트리를 입력받는 방법은 다음과 같다


```python
n = int(input())
hash_table = [set() for _ in range(n+1)]
root_node = None

for _ in range(n-1):
    A, B = map(int, input().split())
    hash_table[A].add(B)
    if not root_node:
        root_node = A
    else:
        if B == root_node:
            root_node = A

print(hash_table)  # hash_table[0] 은 사용하지 않음
print(root_node)  # 부모 노드가 없는 최상위 노드
```

    8
    5 6
    5 7
    5 8
    2 5
    2 4
    1 2
    1 3
    [set(), {2, 3}, {4, 5}, set(), set(), {8, 6, 7}, set(), set(), set()]
    1
    

### DFS (깊이 우선 탐색) 은 Tree 를 Stack 을 사용하여 탐색하는 방식이다
Tree 의 depth, 즉 그림으로 보면 level 이 큰 부분을 먼저 탐색한다


```python
result = []
stack = [root_node]

while stack:
    node = stack.pop()
    result.append(node) 
    
    for vertex in hash_table[node]:
        stack.append(vertex)

print(result) # 동일한 depth 에서는 순서가 바뀔 수 있다
```

    [1, 3, 2, 5, 7, 6, 8, 4]
    

DFS 는 재귀를 사용하여 구현할 수도 있다


```python
result = []

def DFS(node):
    result.append(node)
    for vertex in hash_table[node]:
        DFS(vertex)

DFS(root_node)

print(result) # 동일한 depth 에서는 순서가 바뀔 수 있다
```

    [1, 2, 4, 5, 8, 6, 7, 3]
    

### BFS (너비 우선 탐색) 은 Tree 를 Queue 를 사용하여 탐색하는 방식이다
같은 depth 의 node 를 전부 탐색하고 다음 depth 로 넘어가며 탐색한다


```python
result = []
queue = deque([root_node])

while queue:
    node = queue.popleft()
    result.append(node)
    
    for vertex in hash_table[node]:
        queue.append(vertex)
        
print(result)
```

    [1, 2, 3, 4, 5, 8, 6, 7]
    

DFS 와 BFS 는 꼭 Tree 에서만 사용할 수 있는 것은 아니다 <br>
만약 Graph 에서 적용하는 경우, 방문 처리를 하여 중복으로 탐색되지 않아야 한다 <br>

<hr>

길은 0 으로, 벽은 1 으로 되어 있는 $n×m$ board 가 주어졌을 때 <br>
$(0, 0)$ 부터 $(n-1, m-1)$ 으로 가는 길이 없으면 -1, 있으면 최소 거리를 구하는 알고리즘을 작성해보면 다음과 같다


```python
# board 의 0 을 vertex 라고 생각하면 board 는 graph 라고 볼 수 있다
# 최소 거리를 구하는 것이므로 BFS 로 탐색하여 (n, m) 이 탐색될 때 depth 를 구하면 된다

from collections import deque

def solution(n, m, board):
    if board[0][0] == 1 or board[m-1][n-1] == 1:
        return -1

    is_visited = [[False for _ in range(n)] for __ in range(m)]
    queue = deque([[0, 0]])
    is_visited[0][0] = True
    depth = 0

    while queue:
        count = len(queue)
        for _ in range(count):
            x, y = queue.popleft()
            if x == n-1 and y == m-1:
                return depth

            if x > 0:
                if board[y][x-1] == 0 and (not is_visited[y][x-1]):
                    is_visited[y][x-1] = True
                    queue.append([x-1, y])
            if x < n-1:
                if board[y][x+1] == 0 and (not is_visited[y][x+1]):
                    is_visited[y][x+1] = True
                    queue.append([x+1, y])
            if y > 0:
                if board[y-1][x] == 0 and (not is_visited[y-1][x]):
                    is_visited[y-1][x] = True
                    queue.append([x, y-1])
            if y < m-1:
                if board[y+1][x] == 0 and (not is_visited[y+1][x]):
                    is_visited[y+1][x] = True
                    queue.append([x, y+1])  
        depth += 1
    return -1


n, m = map(int, input().split())
board = []
for _ in range(m):
    board.append(list(map(int, input().split())))
    
print(solution(n, m, board))
```

    7 6
    0 0 0 0 0 0 0
    0 0 0 0 0 1 1
    0 1 1 1 0 0 0
    0 1 1 1 0 1 0
    0 1 0 0 0 1 1
    0 0 0 1 0 0 0
    11
    

# 
