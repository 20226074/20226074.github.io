## module 은 함수를 담은 py 파일이다
- import (module) 의 형태로 불러온다


```python
# 다음은 Module_Test.py 파일의 코드이다

from math import sqrt

def is_coordinate(A):
    if type(A) == list:
        if sum([type(i) == int for i in A]) == len(A):
            return True
    return False

def distance(A, B):
    if is_coordinate(A) and is_coordinate(B):
        if len(A) > len(B):
            B.extend([0]*(len(A)-len(B)))
        elif len(A) < len(B):
            A.extend([0]*(len(B)-len(A)))

        return sqrt(sum([(a-b)**2 for a, b in zip(A, B)]))

print(__name__)
if __name__ == "__main__":  # import 를 하면 이 조건이 부합하지 않는다
    print(distance([1, 2, 3], [3, 4, 4]))
```

    __main__
    3.0
    


```python
import Module_Test
# import 를 하면 그 파일의 코드를 한 번 실행시키는 형식으로 함수를 가져오는 것이다
# Module_Test 코드의 print(__name__) 이 "__main__" 이 아니라 "Module_Test" 를 출력하는 것을 확인할 수 있다

A = [2, 3, 4]
B = [2, 6]
print(Module_Test.is_coordinate(A), Module_Test.is_coordinate(B))
print(Module_Test.distance(A, B))
```

    Module_Test
    True True
    5.0
    


```python
# 똑같은 module 을 다시 import 를 하면 코드 자체를 실행시키지 않으므로, print(__name__) 도 실행되지 않는다
import Module_Test
print(Module_Test.is_coordinate([1, 'a', 3]))
```

    False
    

## package 는 module 을 담은 directory(폴더) 이다
- import (package).(module) 의 형태로 불러온다


```python
# 다음은 single_operator.py 파일의 코드이다

def add(a, b):
    return a + b

def mul(a, b):
    return a * b

if __name__ == '__main__':
    print(add(2, 3), mul(2, 3))
```

    5 6
    


```python
# 다음은 calculator.py 파일의 코드이다

from functools import reduce

def adds(*args):
    return reduce(lambda a,b:a+b, args, 0)

def muls(*args):
    return reduce(lambda a,b:a*b, args, 1)

if __name__ == '__main__':
    print(adds(1, 2, 3, 4))
    print(muls(1, 2, 3, 4))
```

    10
    24
    

- 위의 두 py 파일이 math_package 라는 directory 에 저장되어 있다


```python
import math_package

# package 자체로 import 하는 경우 package 로부터 접근해야 한다
print(math_package.single_operator.add(3, 4))
print(math_package.calculator.adds(2, 3, 4, 5))


from math_package import single_operator
print(single_operator.mul(3, 4))

from math_package.calculator import muls
print(muls(2, 3, 4, 5))
```

    7
    14
    12
    120
    

### 모든 file 은 open function 으로 Input/Output 이 가능하다
- `file = open((file).(extension), (file mode))` 의 형태로 작성하다
  - file 의 작업이 끝나면 `file.close()` 가 필요하다
- `with open(...) as (variable):` 의 형태로 작성하면 body 내에서 파일을 variable 에 저장한다
  - 이 경우 `file.close()` 는 필요하지 않다


```python
f = open("Test.txt", 'w')  # w : 쓰기 (파일이 없으면 생성하며, 있는 경우 기존의 내용은 삭제)
f.write("Hello World!\n")
f.writelines(["One\n", "Two\n", "Three\n"])
f.close()

f = open("Test.txt", 'r')  # r : 읽기 (처음부터 차례대로 불러올 수 있음)
print(f.read(6))
print(f.read()) # 남은 모든 문자열 출력
f.close()

with open("Test.txt", 'r') as f:
    print(f.readline(), end='')
    print(f.readlines())  # 남은 문자열을 줄마다 list 로 받기
    
with open("Test.txt", 'a') as f:  # a : 추가 (파일이 없으면 생성하며, 있는 경우 맨 뒤에서부터 추가)
    f.write("adding...")

f = open("Test.txt", 'x')  # x : 배타적 생성 (파일이 없으면 생성하며 (w), 있는 경우 FileExistsError)

# file mode 뒤에 + 를 붙이면 읽기/쓰기가 모두 가능하다
# file mode 뒤에 t 를 붙이면 text mode 가 된다  ( 읽기/쓰기 모두 가능 )
# file mode 뒤에 b 를 붙이면 binary mode 가 된다
```

    Hello 
    World!
    One
    Two
    Three
    
    Hello World!
    ['One\n', 'Two\n', 'Three\n']
    


    ---------------------------------------------------------------------------

    FileExistsError                           Traceback (most recent call last)

    Input In [19], in <cell line: 18>()
         15 with open("Test.txt", 'a') as f:  # a : 추가 (파일이 없으면 생성하며, 있는 경우 맨 뒤에서부터 추가)
         16     f.write("adding...")
    ---> 18 f = open("Test.txt", 'x')
    

    FileExistsError: [Errno 17] File exists: 'Test.txt'



```python
with open('Module_Test.py', 'r') as f:
    print(f.read())
```

    from math import sqrt
    
    def is_coordinate(A):
        if type(A) == list:
            if sum([type(i) == int for i in A]) == len(A):
                return True
        return False
    
    def distance(A, B):
        if is_coordinate(A) and is_coordinate(B):
            if len(A) > len(B):
                B.extend([0]*(len(A)-len(B)))
            elif len(A) < len(B):
                A.extend([0]*(len(B)-len(A)))
    
            return sqrt(sum([(a-b)**2 for a, b in zip(A, B)]))
    
    print(__name__)
    if __name__ == "__main__":
        print(distance([1, 2, 3], [3, 4, 4]))
    
    


```python
import pickle  # object 를 file 에 저장하기 위해 필요하다

file = open("Testing.pck", "wb")  # Warning : binary mode 로 열어야 한다
pickle.dump(1.23, file)  # file 에 float object 1.23 을 저장한다
pickle.dump([1, 2, 3], file)
file.close()

with open("Testing.pck", 'rb') as file:  # Warning : binary mode 로 열어야 한다
    a = pickle.load(file)  # float object 1.23 을 a 에 assign 한다
    b = pickle.load(file)
    print(a, type(a))
    print(b, type(b))
```

    1.23 <class 'float'>
    [1, 2, 3] <class 'list'>
    

### ※ directory 는 os module 로 수정이 가능하다


```python
import os

print(os.getcwd())
print(os.listdir()[:5])
path = os.getcwd() + '\\math_package'
os.chdir(path)
print(os.getcwd())
print(os.listdir())

os.remove('single_operator.py')
print(os.listdir())

os.mkdir('New_directory')
print(os.listdir())

try:
    os.rmdir('nonempty_directory')
except OSError:
    print("os.rmdir('nonempty_directory') : OSError")
os.rmdir('empty_directory')
print(os.listdir())

path = 'C:\\Users\\oyes2'  # 직접 주소를 입력할 때는 \ 연산에 의해 \\ 로 써줘야 한다 (Data Type 참고)
os.chdir(path)
print(os.getcwd())
```

    C:\Users\oyes2
    ['.gitconfig', '.idlerc', '.ipynb_checkpoints', '.ipython', '.jupyter']
    C:\Users\oyes2\math_package
    ['.ipynb_checkpoints', 'calculator.py', 'empty_directory', 'nonempty_directory', 'single_operator.py', '__pycache__']
    ['.ipynb_checkpoints', 'calculator.py', 'empty_directory', 'nonempty_directory', '__pycache__']
    ['.ipynb_checkpoints', 'calculator.py', 'empty_directory', 'New_directory', 'nonempty_directory', '__pycache__']
    os.rmdir('nonempty_directory') : OSError
    ['.ipynb_checkpoints', 'calculator.py', 'New_directory', 'nonempty_directory', '__pycache__']
    C:\Users\oyes2
    
