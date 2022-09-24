---
layout: single
title: "[Algorithm] Dynamic Programming"
categories: Algorithm
tags: [Algorithm, python]
---


## Dynamic Programming (동적 계획법)
- 이전에 구한 값을 통해서 새로 구하고자 하는 값의 연산을 최적화하는 기법이다
- 단순한 예시로 피보나치 수열을 구하는 알고리즘이 있다


```python
fibonacci = [0, 1]

for _ in range(2, 100):
    fibonacci.append(fibonacci[-1] + fibonacci[-2])

print(fibonacci[95:100])
```

    [31940434634990099905, 51680708854858323072, 83621143489848422977, 135301852344706746049, 218922995834555169026]
    

만약 재귀방식으로 구했으면 $i$번째 수를 구하기 위해서 <br>
$i-1$ 번째 수와 $i-2$ 번째 수를 다시 구해야 하는 상황이 나오게 된다 <br>

<br>
<hr>

위의 예시가 동적계획법으로 작동하는 것에 있어 기준은 항의 순서이다 <br>
$i-1$ 번째 항까지 구했어야 이제 $i$ 번째 항을 빠르게 구할 수 있는 것이다 <br>
따라서 동적계획법은 어떤 기준이 있고, 그 기준에 대하여 구한 다음 <br>
다음 기준으로 넘어갈 때 이전의 기준에서 연산된 것이 사용이 되는 형식이어야 한다 <br>

<hr>

[사칙연산](https://school.programmers.co.kr/learn/courses/30/lessons/1843)
위 문제의 해결 방법으로는 다음과 같다


```python
def solution(arr):
    numbers = list(map(int, arr[0::2])); 
    signs = arr[1::2]
    length = len(numbers)
    
    INF = 1000000
    max_list = [[-INF for _ in range(length)] for _ in range(length)]
    min_list = [[INF for _ in range(length)] for _ in range(length)]
    
    # 기준은 step, 즉 numbers 의 배열에서 구간의 크기로 잡는다
    # 먼저 구간의 크기가 1 일 때에는 숫자가 1개밖에 없으므로 number 그대로를 넣어준다
    for i in range(length):
        max_list[i][i] = numbers[i]
        min_list[i][i] = numbers[i]
    # min_list 를 만드는 이유는, 구간의 크기가 2 이상일 때부터 부호를 감안해야 하는데
    # 두 구간을 이어주는 부호가 - 일 경우, - 뒤에 있는 구간은 min_list 에 있는 값을 사용해야 최대가 된다
    # 물론, min_list 도 부호가 - 이면 - 뒤에 있는 구간은 max_list 에 있는 값을 사용해야 최소가 된다
    
    for step in range(1, length):
        for i in range(length - step):
            j = i+step
            for k in range(i, j):
                if signs[k] == '+':
                    max_list[i][j] = max(max_list[i][j], max_list[i][k] + max_list[k+1][j])
                    min_list[i][j] = min(min_list[i][j], min_list[i][k] + min_list[k+1][j])
                else: # signs[k] == '-':
                    max_list[i][j] = max(max_list[i][j], max_list[i][k] - min_list[k+1][j])
                    min_list[i][j] = min(min_list[i][j], min_list[i][k] - max_list[k+1][j])
    
    return max_list[0][length - 1]
```
