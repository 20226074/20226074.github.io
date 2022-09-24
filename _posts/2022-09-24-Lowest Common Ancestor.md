---
layout: single
title: "[Algorithm] Lowest Common Ancestor"
categories: Algorithm
tags: [Algorithm, python]
---


## Lowest Common Ancestor (최소 공통 조상)
- Tree 가 주어졌을 때 어떤 두 노드에 대하여 가장 인접한 조상이 무엇임을 찾는다
- node 마다 모든 조상을 저장해둔 뒤 Binary Search 를 하면 공간복잡도가 높아지므로  <br>
  $2^i$ 번째 조상만을 저장해둔 뒤 선형 탐색을 하여 Binary Search 를 간접적으로 구현하는 방법을 사용한다
  - $2^i$ 번째 조상을 저장하려면, $2^(i-1)$ 번째 조상의 $2^(i-1)$ 번째 조상을 찾으면 된다
  - 이를 감안하면 Dynamic Programming 으로 $2^i$ 번째 조상들을 $O(N log N)$ 으로 저장할 수 있다
  
[LCA 2](https://www.acmicpc.net/problem/11438) <br>
위 문제의 해결 방법으로는 다음과 같다


```python
import sys
from collections import deque
from math import log2

n = int(sys.stdin.readline())

graph = [[] for _ in range(n+1)]
for _ in range(n-1):
    a, b = map(int, sys.stdin.readline().split())
    graph[a].append(b)
    graph[b].append(a)

binary_max = int(log2(n-1))
parent_table = [[0 for _ in range(binary_max + 1)] for __ in range(n+1)]
# parent_table[node][i] : node 의 2**i 번째 부모
level = [0 for _ in range(n+1)]
visit = [False for _ in range(n+1)]


queue = deque([1])

while queue:
    vertex = queue.popleft()
    visit[vertex] = True
    for node in graph[vertex]:
        if not visit[node]:
            queue.append(node)
            parent_table[node][0] = vertex
            level[node] = level[vertex] + 1

for binary in range(1, binary_max + 1):
    for node in range(1, n+1):
        parent_table[node][binary] = parent_table[parent_table[node][binary - 1]][binary - 1]


m = int(sys.stdin.readline())

for _ in range(m):
    A, B = map(int, sys.stdin.readline().split())

    if level[A] > level[B]:
        A, B = B, A

    temp = level[B] - level[A]
    for i in range(binary_max + 1):
        if temp & (1<<i):
            B = parent_table[B][i]

    if A == B:
        print(A)
        continue

    for i in range(binary_max, -1, -1):
        if parent_table[A][i] != parent_table[B][i]:
            A = parent_table[A][i]
            B = parent_table[B][i]

    print(parent_table[B][0])
print(*reversed(result))
```

    15
    1 2
    1 3
    2 4
    3 7
    6 2
    3 8
    4 9
    2 5
    5 11
    7 13
    10 4
    11 15
    12 5
    14 7
    6
    6 11
    10 9
    2 6
    7 6
    8 13
    8 15
    2
    4
    2
    1
    3
    1
