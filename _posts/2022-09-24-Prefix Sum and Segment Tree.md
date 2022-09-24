---
layout: single
title: "[Algorithm] Longest Incearsing Subsequence"
categories: Algorithm
tags: [Algorithm, python]
---


## Prefix Sum (구간 합)
일반적으로 수열 Numbers 에 대하여 $a$ 번째 수부터 $b$ 번째 수까지의 합을 구하려고 한다면  <br>
$1$ 번째 수부터 $n$ 번째 수까지의 합을 나타내는 배열 Sum 를 만든 다음, Sum[b] - Sum[a-1] 로 구할 수 있다

[구간 합 구하기 4](https://www.acmicpc.net/problem/11659) <br>
위 문제의 해결 코드로는 다음과 같다


```python
import sys

n, m = map(int, sys.stdin.readline().split())

Numbers = list(map(int, sys.stdin.readline().split()))
Sum = [0 for _ in range(n+1)]

for i in range(1, n+1):
    Sum[i] = Sum[i-1] + Numbers[i-1]

for _ in range(m):
    a, b = map(int, sys.stdin.readline().split())
    print(Sum[b] - Sum[a-1])
```

    5 3
    5 4 3 2 1
    1 3
    2 4
    5 5
    12
    9
    1

그런데 만약 중간에 수열의 값이 수정이 될 수 있다고 하면 어떨까? <br>
만약 1번째 값이 수정된다면, Sum 의 모든 원소를 수정해야 하므로 <br>
한 번의 수정에 $O(n)$ 의 시간복잡도를 가지게 된다 <br>

<hr>

Binary Search 와 비슷한 느낌으로 Sum 을 저장하면 <br>
구간 합을 구하는 것은 $O(log N)$ 이 되지만, 수정하는 데에도 $O(log N)$ 이 되도록 만들 수 있다 <br>
구체적으로는 Sum[i] 에 그 $i$의 원소를 포함해서, $i$를 2진수로 나타내었을 때 맨 오른쪽의 $1$이 나타내는 값 <br>
즉, 순서대로 $1$ $2$ $1$ $4$ $1$ $2$ $1$ $8$ $1$ $2$ $1$ $4$ $1$ $2$ $1$ $16$ $1$ .... 의 개수만큼의 합을 저장한다 <br>
그러고 나면 $1$부터 $n$까지의 합을 구할 때, 2진수로 나타내어서 왼쪽부터 차례대로 나타내는 값을 더하면서 <br>
그 때의 Sum 을 더하게 되면 구할 수 있고, 그러면 이전에 보인 예시와 같이 구할 수 있다
그리고 어느 값을 수정하게 될 때에도, 그 값을 포함하는 Sum 만 수정하면 된다

[구간 합 구하기](https://www.acmicpc.net/problem/2042) <br>
위 문제의 해결 코드로는 다음과 같다  ( 다소 체계적이지 않을 수 있다 )


```python
import sys
from math import log2

N, M, K = map(int, sys.stdin.readline().split())

Numbers = [0 for _ in range(N+1)]
Sum = [0 for _ in range(N+1)]
Revise_list = [-1 for i in range(N+1)]

for i in range(1, N+1):
    Numbers[i] = int(sys.stdin.readline())
    Sum[i] = Numbers[i]
    
    step = 2
    while i%step == 0:
        step <<= 1
    step >>= 1
    
    for j in range(int(log2(step))):
        Sum[i] += Sum[i - (1 << j)]
        Revise_list[i - (1 << j)] = i

for _ in range(M+K):
    a, b, c = map(int, sys.stdin.readline().split())
    
    if a == 1:
        diff = c-Numbers[b]
        Numbers[b] = c
        Sum[b] += diff
        
        while Revise_list[b] != -1:
            b = Revise_list[b]
            Sum[b] += diff
        
    elif a == 2:
        if b == 1:
            Sum_b = 0
        else:
            b -= 1
            index = (1 << int(log2(b)))
            Sum_b = Sum[index]
            num = (index >> 1)
        
            while index < b:
                if index + num <= b:
                    index += num
                    Sum_b += Sum[index]
                num >>= 1
            
        index = (1 << int(log2(c)))
        Sum_c = Sum[index]
        num = (index >> 1)
        
        while index < c:
            if index + num <= c:
                index += num
                Sum_c += Sum[index]
            num >>= 1
                
        print(Sum_c - Sum_b)
```

    5 2 2
    1
    2
    3
    4
    5
    1 3 6
    2 2 5
    1 5 2
    2 3 5
    17
    12

이를 좀 더 체계적으로 만들 수 있을까? <br>
Sum 을 List 형태로 저장하지 않고 Tree 형태로 저장할 수 있다 <br>
root node 는 전체의 합을 나타내고, 아래에는 구간을 계속 나누어서 합을 나타내기를 반복한다 <br>
이렇게 데이터의 범위를 분할하여 구간의 통계량을 구하는 데 사용하는 트리를 Segment Tree 라고 한다 <br>

<hr>

동일한 문제에 대한 해결 코드로는 다음과 같다


```python
import sys
from math import log2

N, M, K = map(int, sys.stdin.readline().split())
numbers = [0 for _ in range(N+1)]

for i in range(1, N+1):
    numbers[i] = int(sys.stdin.readline())
    
tree = [0 for _ in range(1 << (int(log2(N)) + 2))]

def create(node, start, end):
    if start == end:
        tree[node] = numbers[start] # numbers[end]
    else:
        tree[node] = create(node*2, start, (start+end)//2) + create(node*2+1, (start+end)//2+1, end)
    return tree[node]

create(1, 1, N)


def Sum(node, start, end, left, right):
    if right < start or end < left:
        return 0
    
    if left <= start and end <= right:
        return tree[node]
    
    return Sum(node*2, start, (start+end)//2, left, right) + Sum(node*2+1, (start+end)//2+1, end, left, right)

def Update(node, start, end, index, diff):
    if index < start or end < index:
        return
    
    tree[node] += diff
    
    if start != end:
        Update(node*2, start, (start+end)//2, index, diff)
        Update(node*2+1, (start+end)//2+1, end, index, diff)

for _ in range(M+K):
    a, b, c = map(int, sys.stdin.readline().split())
    
    if a == 1:
        diff = c - numbers[b]
        numbers[b] = c
        Update(1, 1, N, b, diff)
    else:
        print(Sum(1, 1, N, b, c))
```

    5 2 2
    1
    2
    3
    4
    5
    1 3 6
    2 2 5
    1 5 2
    2 3 5
    17
    12
