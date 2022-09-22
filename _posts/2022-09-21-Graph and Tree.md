---
layout: single
title: "[Algorithm] Graph Theorem"
categories: Algorithm
tags: [Algorithm, python]
---


## 그래프란 vertex 사이가 edge 로 연결된 수학 구조를 말한다
먼저 vertex 의 개수 $n$ 과 간선의 개수 $m$ 이 주어지고 <br>
두 번째 줄부터 간선으로 이어지는 두 개의 vertex 가 차례대로 주어진다고 하자 <br>

<img src="/assets/img/Graph.png">

위의 그래프를 입력받았을 때, 저장하는 방법으로는 크게 2가지가 있다

<br>

### 각 vertex 마다 연결되어 있는 vertex 만을 저장하기

<hr>

```python
n, m = map(int, input().split())
hash_table = [set() for _ in range(n+1)] # 이 list 에 각 vertex 에 대하여 어떤 vertex 와 연결되어 있는지 set 으로 저장한다.

for _ in range(m):
    A, B = map(int, input().split())
    
    hash_table[A].add(B)
    hash_table[B].add(A)

print(hash_table) # hash_table[0] 은 사용하지 않음
```

    6 7
    1 2
    1 4
    2 4
    2 3
    5 4
    3 4
    4 6
    [set(), {2, 4}, {1, 3, 4}, {2, 4}, {1, 2, 3, 5, 6}, {4}, {4}]
    

### 2차원 matrix 형태로 저장하기

<hr>


```python
n, m = map(int, input().split())
matrix = [[0 for _ in range(n+1)] for __ in range(m+1)]

for _ in range(m):
    A, B = map(int, input().split())
    
    matrix[A][B] = 1
    matrix[B][A] = 1
    
print(*matrix, sep='\n') # matrix 의 각 첫 번째 행, 열을 0행, 0열이라고 하고, 0행, 0열은 사용하지 않음
```

    6 7
    1 2
    1 4
    2 4
    2 3
    5 4
    3 4
    4 6
    [0, 0, 0, 0, 0, 0, 0]
    [0, 0, 1, 0, 1, 0, 0]
    [0, 1, 0, 1, 1, 0, 0]
    [0, 0, 1, 0, 1, 0, 0]
    [0, 1, 1, 1, 0, 1, 1]
    [0, 0, 0, 0, 1, 0, 0]
    [0, 0, 0, 0, 1, 0, 0]
    [0, 0, 0, 0, 0, 0, 0]
    

<br>

## 이 그래프의 edge 에 각각 cost 가 생길 경우에도 비슷하게 저장할 수 있다

### 각 vertex 마다 연결되어 있는 vertex 만을 저장하기

<hr>

```python
n, m = map(int, input().split())
hash_table = [set() for _ in range(n+1)] # 이 list 에 각 vertex 에 대하여 어떤 vertex 와 연결되어 있는지 set 으로 저장한다.

for _ in range(m):
    A, B, cost = map(int, input().split())
    
    hash_table[A].add((B, cost))
    hash_table[B].add((A, cost))

print(hash_table) # hash_table[0] 은 사용하지 않음
# hash_table[X] 에는 (Y, cost) 형태의, 연결되어 있는 vertex Y 와 edge 의 cost 가 저장되어 있다
```

    6 7
    1 2 1
    1 4 2
    2 4 3
    2 3 4
    5 4 5
    3 4 6
    4 6 7
    [set(), {(2, 1), (4, 2)}, {(1, 1), (3, 4), (4, 3)}, {(2, 4), (4, 6)}, {(1, 2), (5, 5), (2, 3), (6, 7), (3, 6)}, {(4, 5)}, {(4, 7)}]
    

### 2차원 matrix 형태로 저장하기

<hr>

```python
n, m = map(int, input().split())
matrix = [[0 for _ in range(n+1)] for __ in range(m+1)]

for _ in range(m):
    A, B, cost = map(int, input().split())
    
    matrix[A][B] = cost
    matrix[B][A] = cost
    
print(*matrix, sep='\n') # matrix 의 각 첫 번째 행, 열을 0행, 0열이라고 하고, 0행, 0열은 사용하지 않음
```

    6 7
    1 2 1
    1 4 2
    2 4 3
    2 3 4
    5 4 5
    3 4 6
    4 6 7
    [0, 0, 0, 0, 0, 0, 0]
    [0, 0, 1, 0, 2, 0, 0]
    [0, 1, 0, 4, 3, 0, 0]
    [0, 0, 4, 0, 6, 0, 0]
    [0, 2, 3, 6, 0, 5, 7]
    [0, 0, 0, 0, 5, 0, 0]
    [0, 0, 0, 0, 7, 0, 0]
    [0, 0, 0, 0, 0, 0, 0]
    

### Tree 는 순환이 없는 Graph 이다
그러므로 vertex 가 $n$ 개이면 edge 는 $n-1$ 개이다 <br>
따라서 먼저 vertex 의 개수 $n$ 만 주어지고 <br>
두 번째 줄부터 $n-1$ 개의, ( 부모 vertex, 자식 vertex ) 가 주어진다고 하자 <br>
<img src="/assets/img/Tree.png"> <br>
위의 트리를 입력받는 방법은 다음과 같다


```python
n = int(input())
hash_table = [set() for _ in range(n+1)]  # hash_table[0] 은 사용하지 않음
root_node = None
parent_table = [0 for _ in range(n+1)]  # parent_table[0] 은 사용하지 않음

for _ in range(n-1):
    A, B = map(int, input().split())
    hash_table[A].add(B)
    parent_table[B] = A
    if not root_node:
        root_node = A
    else:
        if B == root_node:
            root_node = A

print(hash_table)
print(root_node)  # 부모 노드가 없는 최상위 노드
print(parent_table)  # 각 노드의 부모 노드
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
    [0, 0, 1, 1, 2, 2, 5, 5, 5]
    
