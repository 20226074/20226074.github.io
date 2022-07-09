### 예약어(Reserved words) 는 특정 기능을 이미 가진 단어를 말한다


```python
import keyword
print(keyword.kwlist)
```

    ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
    


```python
# assert (조건), [에러 메세지]
# 조건이 False 면 AssertionError: [에러 메세지] 를 발생한다

def Test(n):
    assert n < 5, 'n은 5보다 작음을 보장해야 합니다'
    print('check complete')

Test(3)
Test(6)
```

    check complete
    


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    Input In [3], in <cell line: 9>()
          6     print('check complete')
          8 Test(3)
    ----> 9 Test(6)
    

    Input In [3], in Test(n)
          4 def Test(n):
    ----> 5     assert n < 5, 'n은 5보다 작음을 보장해야 합니다'
          6     print('check complete')
    

    AssertionError: n은 5보다 작음을 보장해야 합니다



```python
import asyncio
import time

async def say_after(delay, what): # async : 동시에 작동하는 것을 허용함 (비동기)
    await asyncio.sleep(delay) # await (code) : code 가 실행될 때까지 멈춤 (동기)
    print(what)
    
async def main():
    print("start :", time.strftime("%X"))
    task1 = asyncio.create_task(say_after(1, 'one'))
    task2 = asyncio.create_task(say_after(3, 'three'))
    
    await task1
    await task2
    print("finish :", time.strftime("%x"))

# Jupyter notebook 는 이 코드 자체가 event loop 이다
# 따라서 asyncio.run(main()) 으로는 RuntimeError 가 발생한다
# 하지만 이 코드가 main thread 이기에 event loop 를 새로 얻어서 가동할 수 있다 
loop = asyncio.get_event_loop()  
asyncio.run_coroutine_threadsafe(main(), loop)
```




    <Future at 0x2120321fdc0 state=pending>



    start : 21:15:43
    one
    three
    finish : 07/09/22
    


```python
def Test(n):
    try:
        n = int(n)
        if n < 0:
            raise RuntimeError  # 오류를 발생시킨다
    except ValueError:
        print('ValueError  (정수이어야 한다)')
    except RuntimeError:
        print('RuntimeError (n >= 0 이어야 한다)')
    else:
        print('n + 1 =', n+1)
    finally:
        print('test end')

A = input().split()
for i in A: Test(i)
```

    'asdf' -1 1
    ValueError  (정수이어야 한다)
    test end
    RuntimeError (n >= 0 이어야 한다)
    test end
    n + 1 = 2
    test end
    


```python
word = 'default'

def A():
    word = 'non_global'
A(); print(word)

def B():
    global word
    word = 'global'
B(); print(word)

def C():
    word = 'default'
    def temp():
        word = 'local'
    temp(); print(word)
C()

def D():
    word = 'default'
    def temp():
        nonlocal word
        word = 'nonlocal'
    temp(); print(word)
D()
```

    default
    global
    default
    nonlocal
    


```python
def fibonacci_generator(n):
    result = [1]
    if n >= 2:
        result.append(1)
        for i in range(2, n):
            result.append(result[i-2] + result[i-1])
    
    for i in result:
        yield i

A = fibonacci_generator(10)
print(next(A))
print(A.__next__())

for i in A:
    print(i, end=' ')
```

    1
    1
    2 3 5 8 13 21 34 55 

### 아래는 하나의 주제로 잡기 어려운 것들의 모음이다


```python
import re  # 문자 클래스 [ ] 을 사용하는 등 정규표현식으로 제한하는 module 이다

# re.findall : 주어진 표현과 같은 문자열을 list 로 반환한다
# [^0-9] : 0부터 9 사이의 숫자가 아닌 것과 매치한다  [ 정규표현식 : \D ]
print(re.findall("[^0-9]", 'a1b23c456d7890e'))
print(re.findall("[0-9]+", 'a1b23c456d7890e')) # + : 1번 이상의 패턴을 포함
# ^(문자열) : 맨 앞과 매치한다  /  (문자열)$ : 맨 뒤와 매치한다
print(re.findall("^a", 'a1a313')); print(re.findall("3$", 'a1a313')); print()

# re.fullmatch : 완전하게 같아야 한다
# [ ]{a, b} : [ ] 의 개수가 a~b 인 것과 매치한다  / '.' : 모든 문자
print(re.fullmatch("1[a-z]{2, 4}.", "1abc-"))
print(re.fullmatch("[가-힣][a-zA-Z][0-9]", '가a0')); print()

# re.match : 앞부분이 같아야 한다
print(re.match("[a-z]", 'a1234'))
# re.search : 어느 부분이 같아야 한다
print(re.search("[a-z]", '12a34'))
```

    ['a', 'b', 'c', 'd', 'e']
    ['1', '23', '456', '7890']
    ['a']
    ['3']
    
    None
    <re.Match object; span=(0, 3), match='가a0'>
    
    <re.Match object; span=(0, 1), match='a'>
    <re.Match object; span=(2, 3), match='a'>
    


```python
def decorator1(func):
    def wrapper1():
        print("decorator1")
        func()
        print("Hello")
    return wrapper1

def decorator2(func):
    def wrapper2():
        print("decorator2")
        func()
        print("World")
    return wrapper2

@decorator1
@decorator2
def Test():
    print("Testing")
Test()

# 아래의 코드와 동일하다
# def Test():
#     print("Testing")
# temp = decorator1(decorator2(Test))
# temp()
```

    decorator1
    decorator2
    Testing
    World
    Hello
    
