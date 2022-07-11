---
layout: single
title: "Generator and Coroutine"
categories: basic_programming
tags: [coding, python]
---


## iterator 는 값을 차례대로 꺼낼 수 있는 object 이다
- `__iter__` 가 있는 instance 를 iterable 이라고 하고, object 를 iterator 라고 한다


```python
A = 'asdf1234'.__iter__()
print(A)
print(A.__next__(), next(A))
for i in A:
    print(i, end = ' ')
    if i == 'f': print(); break  # d f 까지만 print
a, b, c, d = map(int, A)
print(a + b + c + d)
try:
    print(next(A))
except StopIteration:
    print("StopIteration")
print(next(A, "StopIteration Exception"))

B = iter(range(5))
print(B)
print(next(B))
for i in B:
    print(i, end = ' ')
print(B[0])
```

    <str_iterator object at 0x00000224E953E7D0>
    a s
    d f 
    10
    StopIteration
    StopIteration Exception
    <range_iterator object at 0x00000224EA2DFB10>
    0
    1 2 3 4 


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    Input In [40], in <cell line: 20>()
         18 for i in B:
         19     print(i, end = ' ')
    ---> 20 print(B[0])
    

    TypeError: 'range_iterator' object is not subscriptable



```python
import math

class iterable_object:
    def __init__(self, end):
        self.current = end/10
        self.end = end
        self.sep = end/10
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= self.end:
            temp = self.current
            self.current += self.sep
            return temp
        else:
            raise StopIteration
            
    def __getitem__(self, index):
        if index < 10:
            return (index+1) * self.sep
        else:
            raise IndexError
            
A = iterable_object(100)
print(next(A))
print(next(A))
for i in A: print(i, end=' ')
print(next(A, '\n'))
print(A[0], A[2], A[4])
```

    10.0
    20.0
    30.0 40.0 50.0 60.0 70.0 80.0 90.0 100.0 
    
    10.0 30.0 50.0
    

<hr>

## generator 는 iterator 를 생성해주는 object 이다
- 예약어 yield (value) : return 과 비슷하지만, 대기 상태를 유지한다
  - object 의 함수에서 return 을 하면, 다시 함수를 실행할 때 처음부터 시작한다
  - 하지만 yield 를 하면, 그 부분부터 중단하고 main 으로 실행을 양보한다 <br>
    ( main 에서 함수를 사용할 때 그 함수로 실행을 양보하는 현상과 동일하다 )
- yield 를 사용한 함수를 할당하여 만들어진다
- yield 로 만들어진 generator 는 **coroutine** 에 속한다
  - `(yield)` : `coroutine.send(value)` 로 양보되면 `(yield)` 에 `value` 의 값이 할당된다
  - `yield from (function)` : sub-generator 로 양보하며, yield 에 걸리면 coroutine 이 아니라 main 으로 양보한다


```python
def sosu():
    sosu_list = []
    num = 2
    while True:
        Check = True
        for i in sosu_list:
            if i*i > num:
                break
            if num%i == 0:
                Check = False
                break
        if Check:
            sosu_list.append(num)
            yield num
        num += 1

A = sosu()  # function() 의 형태이지만 yield 가 있으므로
print(A)    # generator object 로 할당된다
print(dir(A))  # __iter__, __next__ 가 있으므로 iterator 이다
for _ in range(10):
    print(next(A), end=' ')
A.close()  # 대기 상태를 종료한다
A = sosu(); print()
for _ in range(5):
    print(next(A), end=' ')
```

    <generator object sosu at 0x00000224E9E45F50>
    ['__class__', '__del__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__lt__', '__name__', '__ne__', '__new__', '__next__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'close', 'gi_code', 'gi_frame', 'gi_running', 'gi_yieldfrom', 'send', 'throw']
    2 3 5 7 11 13 17 19 23 29 
    2 3 5 7 11 


```python
# generator 표현식은 list 표현식과 괄호만 동일하다
A = (i for i in range(20) if i%4 == 0)  
print(A)
for _ in range(3):
    print(next(A), end=' ')
```

    <generator object <genexpr> at 0x00000224E9E44580>
    0 4 8 


```python
def Test():
    while True:
        print("point 1")
        temp1 = (yield)
        print("point 2")
        temp2 = (yield)
        print("point 3")
        yield temp1
        print("point 4")
        yield temp2
        print("point 5")

A = Test()
print("next(A)")  # main 에서 A 라는 routine 에게 양보
print(next(A))  # temp1 = (yield) 에서 다시 양보
print("A.send(1)") 
print(A.send(1)) # temp1 = (yield) 에 값을 전달하고 temp2 = (yield) 에서 다시 양보
print("next(A)")
print(next(A)) # temp2 = (yield) 에 값을 전달하지 않았으므로, temp2 = None
               # 그리고 yield temp1 에서 temp1 을 main 으로 전달받으며 양보
print("A.send(2)")
print(A.send(2)) # yield temp1 이후부터 시작해서, yield temp2 에 temp 를 main 으로 전달받으며 다시 양보
```

    next(A)
    point 1
    None
    A.send(1)
    point 2
    None
    next(A)
    point 3
    1
    A.send(2)
    point 4
    None
    


```python
def Coroutine():
    total = 0
    while True:
        temp = (yield total)
        total += temp

A = Coroutine()
print(next(A))
print(A.send(1))
print(A.send(2))
print(A.send(3))
```

    0
    1
    3
    6
    


```python
def sub_generator(n):
    total = 0
    for i in range(n):
        total += i
        yield total
        
def Coroutine():
    sum = yield from sub_generator(10)
    print("check")
    
A = Coroutine()
print(next(A))
print(next(A))
print(next(A))
print(next(A))
```

    0
    1
    3
    6
    

## ※ coroutine 은 asyncio module 을 사용하면 비동기 작업이 가능하다
- 두 routine 을 동시에 작동시키는 것이 가능하다
  - asyncio 의 event_loop 내에서 send 를 반복적으로 실행하기 때문에 가능하다
  - @asyncio.coroutine 과 yield from 을 사용한 callback 문으로도 만들 수 있으며, <br>
    async 와 await 를 사용하여도 만들 수 있다


```python
import asyncio
import time

async def say_after(delay, what): # async : 동시에 작동하는 것을 허용함 (비동기)
    await asyncio.sleep(delay)
    print(what)
    
async def main():
    print("start :", time.strftime("%X"))
    task1 = asyncio.create_task(say_after(1, 'one'))
    task2 = asyncio.create_task(say_after(2, 'two'))
    
    await task1
    await task2
    print("finish :", time.strftime("%X"))

# Jupyter notebook 에서는 이 코드 자체가 event loop 이다
# 따라서 asyncio.run(main()) 으로는 RuntimeError 가 발생한다
# 하지만 이 코드가 main thread 이기에 event loop 를 새로 얻어서 가동할 수 있다 
loop = asyncio.get_event_loop()  
asyncio.run_coroutine_threadsafe(main(), loop)
```




    <Future at 0x2323d4d2710 state=pending>



    start : 21:55:22
    one
    two
    finish : 21:55:24
    

### ※ coroutine 의 비동기 작업과 multi thread 의 차이
- coroutine 은 하나의 thread 로 context switching 을 통해 동시성을 보장한다
- python 의 GIL 정책으로 인해 하나의 process 에는 하나의 thread 만 계산하므로 <br>
  multi thread 와 coroutine 의 속도 차이는 없다
  - multi process 를 사용하면 속도 차이를 보인다


```python
import asyncio
import time

async def calculation_coroutine(start, end):
    total = 0
    for i in range(start, end):
        total += i
    print(total)
    
async def main():
    print("start :", time.strftime("%X"))
    task1 = asyncio.create_task(calculation_coroutine(0, 50000000))
    task2 = asyncio.create_task(calculation_coroutine(50000000, 100000000))
    
    await task1
    await task2
    print("finish :", time.strftime("%X"))
    
loop = asyncio.get_event_loop()  
asyncio.run_coroutine_threadsafe(main(), loop)
```




    <Future at 0x2323d4d1960 state=pending>



    start : 22:11:26
    1249999975000000
    3749999975000000
    finish : 22:11:31
    


```python
import threading
import time

def calculation_thread(start, end):
    total = 0
    for i in range(start, end):
        total += i
    print(total)

t1 = threading.Thread(target=calculation_thread, args=(0, 50000000))
t2 = threading.Thread(target=calculation_thread, args=(50000000, 100000000))

print("start :", time.strftime("%X"))

t1.start()
t2.start()
t1.join()
t2.join()

print("finish :", time.strftime("%X"))
```

    start : 22:12:09
    1249999975000000
    3749999975000000
    finish : 22:12:15
    
