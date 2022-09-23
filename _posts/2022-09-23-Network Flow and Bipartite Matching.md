---
layout: single
title: "[Algorithm] Kruskal/Prim and Dijkstra Algorithm"
categories: Algorithm
tags: [Algorithm, python]
---


## Network Flow 는 유향 Graph 에서 간선에 capacity 가 있을 때 최대 유량을 구하는 알고리즘이다
- Edmonds-Karp 알고리즘을 사용해서 구할 것이다
  - 어떤 경로로 source 에서 sink 까지 보낼 수 있으면 보내는 과정을 반복하는데, 음의 유량도 감안한다 <br>
  - 만약 어느 경로에서 덜 나오게 된다면 source 에서 더 나오게 될 수 있는 경우를 감안하기 위해서이다 <br>


<img src="/assets/img/Network_Flow.png">  <br>

위와 같은 Graph 를 입력했을 때 알고리즘은 다음과 같다


```python
from collections import deque

INF = 100000000
n, m = map(int, input().split())

input_list = []
unique_set = set()

for _ in range(m):
    input_list.append(input().split())
    unique_set.add(input_list[-1][0])
    unique_set.add(input_list[-1][1])

capacity = {start:{end:0 for end in unique_set} for start in unique_set}  # 어떤 간선의 용량
flow = {start:{end:0 for end in unique_set} for start in unique_set}  # 실제로 간선에 흐르는 양
adjList = {start:[] for start in unique_set}

for start, end, cap in input_list:
    capacity[start][end] = int(cap)
    adjList[start].append(end)
    adjList[end].append(start)

source, sink = input().split()
result_flow = 0

while True:
    # BFS 로 먼저 flow 할 간선을 정한다  [ visit 로 마치 queue 처럼 저장한다 ]
    visit = {node:'' for node in unique_set}
    queue = deque([source])

    while queue and visit[sink] == '':
        start = queue.popleft()

        for end in adjList[start]:
            # capacity 보다 flow 가 커서 아직 흐를 수 있고, 방문한 적이 없으면 그 vertex 로 간선을 정한다
            if capacity[start][end] - flow[start][end] > 0 and visit[end] == '':
                queue.append(end)
                visit[end] = start  # 이렇게 저장하면 flow 를 채울 때 왔던 간선들을 되돌아갈 수 있다

                if end == sink:
                    break

    if visit[sink] == '':  # 만약 더 이상 sink 까지 flow 를 흐르게 만들 수 없는 경우
        break
    else:
        min_flow = INF

        node = sink
        while node != source:
            min_flow = min(min_flow, capacity[visit[node]][node] - flow[visit[node]][node])
            node = visit[node]

        node = sink
        while node != source:
            flow[visit[node]][node] += min_flow
            flow[node][visit[node]] -= min_flow
            node = visit[node]

        result_flow += min_flow

print(result_flow)
```

    6 10
    1 2 14
    1 4 12
    2 4 14
    2 5 6
    4 5 11
    2 3 5
    5 3 4
    3 6 8
    5 6 7
    2 6 10
    1 6
    25
    

<br> <hr>

## Bipartite Matching (이분 매칭) 은 두 그룹의 원소를 최대한 Matching 시키는 알고리즘이다
- Start 와 End node 를 추가하면 모든 간선의 용량이 1인 네트워크 플로우로 볼 수 있다
- 하지만 Edmonds-Karp 알고리즘을 사용하진 않고, 단순히 BFS 를 통해 더 효율적으로 구할 수 있다


<img src="/assets/img/Network_Flow.png">  <br>

위와 같은 Graph 를 입력했을 때 알고리즘은 다음과 같다


```python
n, m, k = map(int, input().split())  # n, m 은 각 Group 의 vertex 의 개수이며, k 는 간선의 개수이다

edges = {}
childs = {}
parents = {}

for _ in range(k):
    start, end = input().split()
    if start not in edges:
        edges[start] = [end]
    else:
        edges[start].append(end)
    
for start in edges:
    for end in edges[start]:
        if end not in childs.values():
            childs[start] = end
            parents[end] = start
            break
    
    if start in childs:
        continue
    
    for end in edges[start]:
        for end_end in edges[parents[end]]:
            if end_end not in childs.values():
                childs[end] = end_end
                parents[end_end] = end
                childs[start] = end
                parents[end] = start
                break
        
        if end in childs.values():
            break

print(childs)
```

    5 5 8
    A 1
    A 3
    B 1
    B 2
    C 5
    D 3
    E 2
    A 5
    {'A': '1', 'B': '2', 'C': '5', 'D': '3'}
    
