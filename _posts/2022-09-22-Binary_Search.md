---
layout: single
title: "[Algorithm] Binary Search"
categories: Algorithm
tags: [Algorithm, python]
---


### Binary Search 는 정렬된 List 에서 원하는 원소를 찾는 알고리즘이다
- 정렬되어 있으면 원하는 원소가 지금 고른 원소와의 대소를 판단할 수 있다 
- 따라서 max 와 min 이 있으면, mid 을 잡아서 대소를 판단하고 <br>
  mid 가 더 크면 mid 를 max 로, mid 가 더 작으면 mid 를 min 으로 잡는 과정을 반복해서 구한다 <br>
  ( 상황에 따라 max-1, min+1 로 하는 등 처리가 다를 수 있다 )
- 그러므로 $O(log N)$ 의 시간복잡도로 처리한다


```python
def binary_search(arr, value):
    min_index = 0
    max_index = len(arr) - 1
    while min_index != max_index:
        mid_index = (min_index + max_index) // 2
        if arr[mid_index] == value:
            return mid_index
        if arr[mid_index] > value:
            max_index = mid_index - 1
        else: # arr[mid_index] < value
            min_index = mid_index + 1
    if arr[min_index] == value: # arr[max_index] == value
        return min_index  # max_index
    else:
        return -1

print(binary_search([2, 3, 5, 7, 11, 13, 17, 19, 23, 29], 19))    
```

    7
    

python 에서는 bisect 라이브러리를 사용해 Binary Search(Bisection) 을 할 수 있다


```python
import bisect

List = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]

# bisect 로 나오는 값은 List 의 정렬이 유지되도록 value 를 넣을 때의 index 이다
print(bisect.bisect(List, 12)) # lo=0; hi=9  /  bisect 는 bisect_right 과 동일하다

# 만약 List 에 같은 값이 있을 경우, bisect_left 는 그 값의 index,
# bisect_right 는 그 값의 바로 오른쪽의 index 가 나온다
print(bisect.bisect_left(List, 13))
print(bisect.bisect_right(List, 13))
```

    5
    5
    5
    6
    

<br>

Binary Search 는 꼭 무언가 형상화된 List 에 대하여 원하는 값의 index 를 찾는 것만은 아니다 <br>
어느 정수 범위에 원하는 값이 있고, 어느 값이 원하는 값과의 대소를 판단할 수 있으면 사용할 수 있다 <br>


[징검다리](https://school.programmers.co.kr/learn/courses/30/lessons/43236)  <br>

위 문제의 해결 코드로는 다음과 같다


```python
def solution(distance, rocks, n):
    rocks = [0] + sorted(rocks) + [distance]
    steps = [rocks[i+1] - rocks[i] for i in range(len(rocks) - 1)]
    
    # 구하고자 하는 것은 step 들의 최소값이 최대가 되도록 n개의 바위를 제거했을 때 그 최소값이 무엇인가이다
    # 한편, 바위를 n개 제거하면 step 의 개수는 len(steps)-n 이고, 이 때 최소값이 최대가 되려면
    # 최대한 균등하게 바위가 놓여져 있어야 하므로 distance//(len(steps)-n) 이하일 것이다
    # 따라서, step 의 최소값은 1 이상 distance//(len(steps)-n) 이하의 정수이다
    # 한편, step 의 최소값이 어떤 정수 K 라고 가정을 하면, 그것에 맞춰서 바위를 제거할 수 있다
    # [ 하나의 간격이 K 이상이 될 때까지 바위를 제거하는 방식으로 한다 ]
    # 그렇게 바위를 제거했을 때 만약 n개 이상을 제거했더라면, step 의 최소값은 더 작은 것이고, n개 이하를 제거했더라면, step 의 최소값은 더 클 것이다
    # 이렇게 어떤 값에 대하여 원하는 값과의 대소관계를 파악할 수 있으므로, binary search 를 사용할 수 있다
    
    min_step = 1; max_step = distance//(len(steps)-n)
    
    while min_step + 1 < max_step:
        mid_step = (max_step + min_step)//2
        temp = 0; count = 0
        for step in steps:
            temp += step
            if temp < mid_step:
                count += 1
                if count > n:
                    break
            else:
                temp = 0
        if count > n: # 무조건 count <= n 인 step 을 선택해야 하므로, 마지막에는 min_step 을 선택한다
            max_step = mid_step
        else: # count <= n:
            min_step = mid_step
        
    return min_step

print(solution(25, [2, 14, 11, 21, 17], 2))
```

    4
    
