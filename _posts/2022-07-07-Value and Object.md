---
layout: single
title: "Value and Object"
categories: basic_programming
tags: [coding, python]
---


## int, str 와 같은 type 은 value 이다
- 두 변수의 값이 같다면, identity 도 역시 같다


```python
a = 1; b = 1
print(id(a))
print(id(b))
print(a == b, a is b); print()

a = 'asdf'; b = 'asdf'
print(id(a))
print(id(b))
print(a == b, a is b); print()
```

    1827839934704
    1827839934704
    True True
    
    1827945343728
    1827945343728
    True True
    
    


```python
a = 'asdf'
b = a
print(id(a))
print(id(b))
print(a == b, a is b)

a = 'change'; print(a)
print(b)
```

    1827945343728
    1827945343728
    True True
    change
    asdf
    

<hr>

## list, dictionary 와 같은 type 은 object 이다
- 두 변수의 값이 같아도 identity 는 직접 할당하지 않으면 같지 않다
- class 를 새로 정의할 경우 일반적으로 object 이다
- object 를 가지면서 idnetity 가 같은 두 변수에 대해 <br>
  그 object 를 item assignment 를 하면 두 변수 모두 수정된다  ( Aliasing )
  - 함수에서 받는 매개변수에서도 마찬가지이다 <br>
    ( 그러기에 class 에서 self 를 수정하면 instance 가 수정되는 것이다 )


```python
A = [1, 2, 3]
B = [1, 2, 3]
print(id(A))
print(id(B))
print(A == B, A is B); print()

A = {'a':1, 'b':2}
B = {'a':1, 'b':2}
print(id(A))
print(id(B))
print(A == B, A is B); print()

class Coordinate:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return "({}, {})".format(x, y)
    
    def __eq__(self, other):
        return True if (self.x == other.x and self.y == other.y) else False

A = Coordinate(1, 2)
B = Coordinate(1, 2)
print(id(A))
print(id(B))
print(A == B, A is B)
```

    1827945687104
    1827945687872
    True False
    
    1827945623104
    1827944801920
    True False
    
    1827917368432
    1827917250864
    True False
    


```python
A = [1, 2, 3]
B = A
print(id(A))
print(id(B))
print(A == B, A is B)

A[0] = 3; del A[2]
print(A)
print(B)

def Test(temp):
    temp[0] = 1
    print(temp)

Test(A)
print(A)
print(B)
```

    1827945621568
    1827945621568
    True True
    [3, 2]
    [3, 2]
    [1, 2]
    [1, 2]
    [1, 2]
    

<hr>
<br>

## copy module 을 사용하여 object 를 다른 id 로 복사할 수 있다
- copy.copy() 를 사용하면 object 의 요소를 각각 가져와서 서로 다른 id 를 만들어낸다
  - 2차원 이상의 경우 object 의 요소가 object 이므로, 그 요소 object 의 수정은 공유된다
- copy.deepcopy() 를 사용하면 object 자체를 복사해서 서로 다른 id 를 만들어낸다


```python
import copy

a = [1, 2, 3, [4, 5, 6]]
b = copy.copy(a)  # a.copy() 와 동일하다
print(id(a))
print(id(b))
print(a == b, a is b)
a[0] = 0
print(a)
print(b)
a[3][0] = 0
print(a)
print(b); print()
```

    1827945612928
    1827945624768
    True False
    [0, 2, 3, [4, 5, 6]]
    [1, 2, 3, [4, 5, 6]]
    [0, 2, 3, [0, 5, 6]]
    [1, 2, 3, [0, 5, 6]]
    
    


```python
a = [1, 2, 3, [4, 5, 6]]
b = copy.deepcopy(a)
print(id(a))
print(id(b))
print(a == b, a is b)
a[0] = 0
print(a)
print(b)
a[3][0] = 0
print(a)
print(b); print()
```

    1827944825088
    1827945610880
    True False
    [0, 2, 3, [4, 5, 6]]
    [1, 2, 3, [4, 5, 6]]
    [0, 2, 3, [0, 5, 6]]
    [1, 2, 3, [4, 5, 6]]
    
    

### ※ list 에서 operator * 의 연산 방법
- list A 에 대하여 A * 10 의 의미는 A 의 요소를 10번 extend 한 list 를 말한다
- 따라서 A 의 요소에 object 가 있다면 같은 id 로 복사된다
- 한편, `List = [i for i in range(5)]` 으로 하면 List 에는 `[1, 2, 3, 4, 5]` 가 저장된다
- 만약, `List = [(object) for _ in range(5)]` 와 같이 정의가 되면, for 은 반복문이기에 <br>
  하나의 요소를 만들 때마다 object 를 재정의하기에 다른 id 로 복사된다 


```python
A = [[0] * 5] * 5
B = [[0 for _ in range(5)]] * 5
C = [[0] * 5 for _ in range(5)]
D = [[0 for i in range(5)] for j in range(5)]

A[0][0] = 1; B[0][0] = 1; C[0][0] = 1; D[0][0] = 1
print(A)  # [[*]*] : 같은 id 로 복사
print(B)  # [[for]*] : 같은 id 로 복사
print(C)  # [[*]for] : 다른 id 로 복사
print(D)  # [[for]for] : 다른 id 로 복사
```

    [[1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0]]
    [[1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0], [1, 0, 0, 0, 0]]
    [[1, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]]
    [[1, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]]
    
