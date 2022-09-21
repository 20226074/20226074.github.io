---
layout: single
title: "[Algorithm] Graph Theorem"
categories: Algorithm
tags: [Algorithm, python]
---


## 그래프란 vertex 사이가 edge 로 연결된 수학 구조를 말한다
첫째 줄에 vertex 의 개수 $n$ 과 간선의 개수 $m$ 이 주어지고 <br>
두 번째 줄부터 간선으로 이어지는 두 개의 vertex 가 차례대로 주어진다고 하자 <br>

<img src="/assets/img/Graph.png"> (source : wikipedia)

#### 위의 그래프를 입력받았을 때, 저장하는 방법으로는 크게 2가지가 있다 <br>

<hr> <br>
### 각 vertex 마다 연결되어 있는 vertex 만을 저장하기


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
    

## 이 그래프의 edge 에 각각 cost 가 생길 경우에도 비슷하게 저장할 수 있다

### 각 vertex 마다 연결되어 있는 vertex 만을 저장하기


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
    
