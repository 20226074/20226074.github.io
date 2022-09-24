---
layout: single
title: "[Algorithm] Kruskal/Prim and Dijkstra Algorithm"
categories: Algorithm
tags: [Algorithm, python]
---


## Kruskal/Prim Algorithm 은 가중 그래프의 모든 vertex 를 포함하는 Tree (최소 신장 트리) 를 찾는 방법이다
- Kruskal 은 가장 작은 cost 의 edge 부터 포함시켜서 Tree 를 만든다 <br>
  - edge 를 추가시킬 때 순환이 되는지 아닌지는 Tree 의 부모가 같은지 다른지를 확인하여 알 수 있다
- Prim 은 하나의 vertex 를 tree 로 두고, tree 에 있는 vertex 중 edge 중 가장 작은 cost 의 edge 를 포함시키는 과정을 반복하여 찾는다
  - edge 를 추가시킬 때 순환이 되는지 아닌지는 vertex 를 둘 다 가지고 있는지 아닌지를 확인하여 알 수 있다
  - edge 를 Heap 에 넣어서 사용하면 $O(E log V)$ 의 시간복잡도로 처리할 수 있다


<img src="/assets/img/Graph_example1.png">

Kruskal 을 사용하기 위해, 위의 그래프를 다음과 같이 저장하자

```python
n, m = map(int, input().split())
edge_list = []

for _ in range(m):
    A, B, cost = input().split()
    edge_list.append([A, B, int(cost)])

print(edge_list)
```

    7 11
    A B 7
    A D 5
    B D 9
    B E 7
    B C 8
    C E 5
    D E 15
    D F 6
    E F 8
    E G 9
    F G 11
    [['A', 'B', 7], ['A', 'D', 5], ['B', 'D', 9], ['B', 'E', 7], ['B', 'C', 8], ['C', 'E', 5], ['D', 'E', 15], ['D', 'F', 6], ['E', 'F', 8], ['E', 'G', 9], ['F', 'G', 11]]
    

이 Graph 에서 Kruskal 으로 최소 신장 트리를 찾으면 다음과 같다


```python
edge_list = sorted(edge_list, key=lambda x: x[2])
parents = {}
cost = 0
count_root_node = 0

for i in range(m):
    if len(parents) == n and count_root_node == 1:
        break
    
    if edge_list[i][0] not in parents and edge_list[i][1] not in parents:
        parents[edge_list[i][0]] = edge_list[i][0]
        parents[edge_list[i][1]] = edge_list[i][0]
        count_root_node += 1
    elif edge_list[i][0] not in parents:
        parents[edge_list[i][0]] = edge_list[i][1]
    elif edge_list[i][1] not in parents:
        parents[edge_list[i][1]] = edge_list[i][0]
    else:
        A = edge_list[i][0]; B = edge_list[i][1]
        while parents[A] != A:
            A = parents[A]
        while parents[B] != B:
            B = parents[B]
        if A == B:
            continue
        else:
            parents[A] = B
            count_root_node -= 1

    cost += edge_list[i][2]

print(cost)
```

    39


<br>

Prim 을 사용하기 위해, 위의 그래프를 다음과 같이 저장하자

```python
n, m = map(int, input().split())
hash_table = {}

for _ in range(m):
    A, B, cost = input().split()
    if A not in hash_table:
        hash_table[A] = [[int(cost), B]]
    else:
        hash_table[A].append([int(cost), B])
    if B not in hash_table:
        hash_table[B] = [[int(cost), A]]
    else:
        hash_table[B].append([int(cost), A])

print(hash_table)
```

    7 11
    A B 7
    A D 5
    B D 9
    B E 7
    B C 8
    C E 5
    D E 15
    D F 6
    E F 8
    E G 9
    F G 11
    {'A': [[7, 'B'], [5, 'D']], 'B': [[7, 'A'], [9, 'D'], [7, 'E'], [8, 'C']], 'D': [[5, 'A'], [9, 'B'], [15, 'E'], [6, 'F']], 'E': [[7, 'B'], [5, 'C'], [15, 'D'], [8, 'F'], [9, 'G']], 'C': [[8, 'B'], [5, 'E']], 'F': [[6, 'D'], [8, 'E'], [11, 'G']], 'G': [[9, 'E'], [11, 'F']]}

    
이 Graph 에서 Prim 으로 최소 신장 트리를 찾으면 다음과 같다


```python
import heapq

vertex = list(hash_table.keys())[0]
vertexs = [vertex]
edges = []
for edge in hash_table[vertex]:
    heapq.heappush(edges, edge)
cost = 0

while len(vertexs) < n:
    edge_cost, vertex = heapq.heappop(edges)
    
    if vertex not in vertexs:
        vertexs.append(vertex)
        for edge in hash_table[vertex]:
            heapq.heappush(edges, edge)
        cost += edge_cost

print(cost)
```

    39


<br>

## Dijkstra 알고리즘은 vertex 간의 최소 거리를 찾는 알고리즘이다
- prim 과 비슷하게 접근하지만, 처음 vertex 가 아닌 vertex 는 그 vertex 까지 가는 거리를 포함한다
- cost 를 기준으로 하는 BFS 라고 볼 수 있다

Prim 과 동일하게 그래프를 입력받았을 때, 'A' 에서 다른 vertex 까지의 최단 거리를 찾으면 다음과 같다


```python
import heapq

INF = 100000000
distances = {node:INF for node in hash_table}
distances['A'] = 0
queue = []
heapq.heappush(queue, [distances['A'], 'A'])

while queue: 
    current_distance, current_destination = heapq.heappop(queue)

    if distances[current_destination] < current_distance:
        continue
    
    for new_distance, new_destination in hash_table[current_destination]:
        distance = current_distance + new_distance 
        if distance < distances[new_destination]: 
            distances[new_destination] = distance
            heapq.heappush(queue, [distance, new_destination]) 

print(distances)
```

    {'A': 0, 'B': 7, 'D': 5, 'E': 14, 'C': 15, 'F': 11, 'G': 22}
    
