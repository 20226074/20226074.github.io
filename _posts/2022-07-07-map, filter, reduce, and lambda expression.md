---
layout: single
title: "map, filter, reduce, and lambda expression"
categories: basic_programming
tags: [coding, python]
---


## map 은 iterable object 의 요소를 function 에 따라 변환시킨다
- function 은 iterable object 의 요소를 넣었을 때 동작되도록 해야 한다
- map 으로 반환되는 것은 map object 이므로, 상황에 따라 type 변환이 필요하다


```python
def square(num):
    return num**2

A = map(square, [1, 2, 3, 4, 5])
print(A)
A = list(A)
print(A)
```

    <map object at 0x00000202D886D990>
    [1, 4, 9, 16, 25]
    


```python
A = map(int, input().split())
print(sum(A))  # map object 도 iterable object 이다
```

    1 2 3
    6
    

<hr>

## filter 는 iterable object 의 요소를 function 에 따라 제거시킨다
- function 은 iterable object 의 요소를 넣었을 때 True/False 를 반환하도록 해야 한다
- True 가 나온 요소는 두고, False 가 나온 요소는 제거시킨다
- filter 으로 반환되는 것은 filter object 이므로, 상황에 따라 type 변환이 필요하다


```python
def check_odd(num):
    return num % 2 == 1

A = filter(check_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
print(A)
A = list(A)
print(A)
```

    <filter object at 0x00000202D88FEE60>
    [1, 3, 5, 7, 9]
    


```python
A = list(filter(bool, [1, 2, 0, '', None, 3, 4]))
print(A)
```

    [1, 2, 3, 4]
    

<hr>

## reduce 는 iterable object 의 요소를 function 에 따라 누적시킨다
- `import functools` 이 필요하며, `functools.reduce(function, iterable, intializer)` 로 작성한다
- function 은 intializer 와 iterable object 의 요소를 넣었을 때 동작되도록 해야 한다
- reduce 으로 반환되는 것은 iterable object 의 요소의 type 이다


```python
from functools import reduce

def Max(a, b):
    return a if a > b else b

A = reduce(Max, [1, 5, 2, 4, 3], 0)
print(A)
```

    5
    


```python
import operator

A = reduce(operator.mul, [1, 2, 3, 4, 5], 1)
print(A)
B = reduce(operator.add, [['a', 'b'], ['c', 'd'], ['e', 'f']])
print(B)
```

    120
    ['a', 'b', 'c', 'd', 'e', 'f']
    

<br>
<hr>

## lambda expression 은 function 을 빠르게 적는 방식이다
- `lambda (bound variable): (Body)` 의 형태로 작성된다
- lambda expression 으로 표현된 함수는 계속 저장되지 않는다
  - map, filter, reduce 에서 1회성으로 함수가 필요할 경우 lambda 를 사용할 수 있다
  - 저장하려고 한다면 변수에 저장은 가능하지만 보통 사용하진 않는다


```python
A = list(map(lambda n: n-1, [1, 2, 3, 4, 5]))
print(A)
B = list(filter(lambda n: n>3, [1, 2, 3, 4, 5]))
print(B)
C = reduce(lambda a,b: a*b+1, [1, 2, 3, 4, 5], 1)
print(C)
```

    [0, 1, 2, 3, 4]
    [4, 5]
    326
    


```python
sqaure = lambda n: n**2
print(sqaure(2))
```

    4
    
