---
layout: single
title: "[Algorithm] Kruskal/Prim and Dijkstra Algorithm"
categories: Algorithm
tags: [Algorithm, python]
---


## Longest Increasing Subsequence (LIS) (최장 증가 부분 수열)
- 어떤 수열이 있을 때 필요한 만큼 수를 제거하여 만들어지는 증가 수열 중 길이의 최대를 구한다
- $i$ 개로 이루어진 증가 수열을 만들었을 때 마지막 수의 최소값을 저장하면 <br>
  이 저장한 값을 기준으로 Binary Search 를 통해 Dynamic Programming 으로 구현할 수 있다
  
[가장 긴 증가하는 부분 수열 5](https://www.acmicpc.net/problem/14003) <br>
위 문제의 해결 방법으로는 다음과 같다


```python
import bisect

n = int(input())

numbers = map(int, input().split())

INF = 1000000001
min_values = [-INF]  # min_values[i] : 앞의 수 중 i개의 수로 IS 를 만들었을 때 마지막 수가 될 수 있는 것 중 가장 작은 수 
logs = []

for number in numbers:
    index = bisect.bisect_left(min_values, number)
    if index == len(min_values):
        min_values.append(number)
    else:
        min_values[index] = number
    
    logs.append([index, number])

index = len(min_values) - 1
print(index)

result = []
for i in range(n-1, -1, -1):
    if logs[i][0] == index:
        result.append(logs[i][1])
        index -= 1

print(*reversed(result))
```

    6
    10 20 10 30 20 50
    4
    10 20 30 50
    
