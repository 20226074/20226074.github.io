### 함수는 연산을 정의하는 것이다
- `def (함수명((매개변수))` 의 형태로 정의된다
  - `*args` : 매개변수의 개수 제한 없이 받으며, tuple 로 저장한다
  - `**kwarg` : 매개변수를 key=value 형태로 무한정 받으며, dictionary 로 저장한다


```python
def Test(name, *args, **kwarg):
    print(name)
    print(args)
    print(kwarg)

Test('A', 'B', 'D', C='restart', F='fail')
```

    A
    ('B', 'D')
    {'C': 'restart', 'F': 'fail'}
    

### Class 는 Object 를 만들기 위한 것이다

- class A 로 정의된 변수는 object 이자 A 의 instance 이다
- class A 에서 self 는 A 의 instance 를 말한다


```python
def rating_test(A, B):  # A, B : Review object
    avgA = sum(A.rating)/len(A.rating)
    avgB = sum(B.rating)/len(B.rating)
    if avgA < avgB:
        return '<'
    elif avgA > avgB:
        return '>'
    else:
        return '='

class Review:
    
    # 변수에 initialize 할 때 적용된다  [ A = Review(...) ]
    def __init__(self, name, rating=[0], description=['None']):
        self.name = name
        self.rating = rating if type(rating) == list else [rating]
        self.description = description if type(description) == list else [description]
    
    # 객체를 나타내는 공식적인 문자열을 출력한다  [ repr(A) ]
    def __repr__(self):
        return "Review({}, {}, {})".format(self.name, self.rating, self.description)
    
    # 객체를 이해하기 쉽게 표현하는 문자열을 출력한다.  [ str(A) ]
    def __str__(self):
        return "{}({}) : {}".format(self.name, self.rating, self.description)
    
    # 객체의 길이를 반환한다  [ len(A) ]
    def __len__(self):
        return len(self.rating)
    
    # 객체의 iterator 를 반환한다  [ for i in A 와 같이 사용할 경우 ]
    def __iter__(self):
        return iter(self.rating)
    
    # 객체에 없는 속성을 참조하려고 할 때 호출된다  [ print(A.age) 와 같은 경우 ]
    def __getattr__(self, name):
        print("속성은 오직 name, rating, description 만 있습니다")
        
    # 객체의 속성을 변경할 때 호출된다  [ setattr(object, name, value) 가 포함되어야 한다 ]
    def __setattr__(self, name, value):
        if name == 'rating':
            if type(value) == int:
                value = [value]
            
            for i in range(len(value)):
                if type(value[i]) != int and type(value[i]) != float:
                    value[i] = 0
                elif 0 > value[i] or value[i] > 5:
                    value[i] = 0
        
        super().__setattr__(name, value) # recursion 을 피하기 위해 super() 을 사용한다
    
    # 객체의 값을 참조할 때 호출된다  [ getattr(object, name) 이 포함되어야 한다 ]
    def __get__(self, instance, owner):
        return [getattr(instance, 'name'), getattr(instance, 'rating'), getattr(instance, 'description')]
    
    # 객체의 값을 변경할 때 호출된다  [ setattr(object, name, value) 가 포함되어야 한다 ]
    # def __set__(self, instance, value):
    
    # 객체가 [ ] 연산자로 참조할 때 동작을 정의한다  [ print(A[0]) 와 같은 경우 ]
    def __getitem__(self, key):
        if type(key) == int:
            if 0 <= key and key < len(self):
                return [self.name, self.rating[key], self.description[key]]
        else:
            print("key is wrong")
    
    # x < y 를 정의한다
    def __lt__(self, other):
        result = rating_test(self, other)
        if result == '<':
            return True
        else:
            return False
    
    # x <= y 를 정의한다
    def __le__(self, other):
        result = rating_test(self, other)
        if result == '<' or result == '=':
            return True
        else:
            return False
    
    # x > y 를 정의한다
    def __gt__(self, other):
        result = rating_test(self, other)
        if result == '>':
            return True
        else:
            return False

    # x >= y 를 정의한다
    def __ge__(self, other):
        result = rating_test(self, other)
        if result == '>' or result == '=':
            return True
        else:
            return False
        
    # x == y 를 정의한다
    def __eq__(self, other):
        result = rating_test(self, other)
        if result == '=':
            return True
        else:
            return False
        
    # x != y 를 정의한다
    def __ne__(self, other):
        result = rating_test(self, other)
        if result == '<' or result =='>':
            return True
        else:
            return False
        
        
    # x + y 를 정의한다  ( x, y : Review object )
    # 지금은 x, y 가 모두 Review object 라서 __add__ 만 정의해도 된다
    # 하지만 다르다면 역순의 경우 __radd__ 를 정의해야 한다
    def __add__(self, other):
        if self.name == other.name:
            self.rating.extend(other.rating)
            self.description.extend(other.description)
            return self
        else:
            print("name is different")
```


```python
a = Review('Top gun', [2, -100], ['soso', 'what the...'])
b = Review('Top gun', [3, 4], ['not bad', 'great'])

print(repr(a))
for i in a:
    print(i)
print(a[0])
print(a < b)
print(a + b)
```

    Review(Top gun, [2, 0], ['soso', 'what the...'])
    2
    0
    ['Top gun', 2, 'soso']
    True
    Top gun([2, 0, 3, 4]) : ['soso', 'what the...', 'not bad', 'great']
    

### Inheritance(상속) 은 부모 클래스의 함수를 사용할 수 있게 하는 것이다

- `class Child(Parent1, Parent2, ...)` 의 형식으로 상속한다 <br>
- 상속하지 않아도 명시적 참조로 `Parent.(함수명)` 을 사용할 수 있지만 <br>
  상속하였다면 간접 참조로 `super(Child, self).(함수명)` 을 쓰는 것이 유용하다
  - `super()` 를 사용하면 다중 상속의 경우 모든 Parents 의 함수를 참조할 수 있다
  - `super()` 를 사용하면 종속성을 바르게 나타내어 MRO 의 관계가 뚜렷하게 나타난다


```python
from math import sqrt

class Parent:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return "({}, {})".format(self.x, self.y)
    
    def distance(self, other):
        return sqrt((self.x - other.x)**2 + (self.y - other.y)**2)
    
class Child(Parent):
    def __init__(self, x, y):
        super(Child, self).__init__(x, y)
    
    def scalar(self):
        return sqrt(self.x**2 + self.y**2)
    
a = Child(3, 4)
b = Child(9, 12)
print(a.scalar())
print(a.distance(b))
```

    5.0
    10.0
    

#### ※ settatr 의 재정의 방법
- `__setattr__` 는 객체의 속성을 변경할 때 호출된다
- 하지만 만약 속성을 변경할 때 감안하고 싶은 것이 있어 `__setattr__` 를 수정할 때 <br>
  `setattr(self, name, value)` 를 사용하게 되면 다시 `__setattr__` 가 실행되므로 <br>
  `RecursionError` 가 발생한다


```python
class Test:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return "({}, {})".format(self.x, self.y)
   
    def __setattr__(self, name, value):
        if name == 'x':
            value = abs(value)
        setattr(self, name, value)

A = Test(-1, 2)
print(A)
```


    ---------------------------------------------------------------------------

    RecursionError                            Traceback (most recent call last)

    Input In [25], in <cell line: 14>()
         11             value = abs(value)
         12         setattr(self, name, value)
    ---> 14 A = Test(-1, 2)
         15 print(A)
    

    Input In [25], in Test.__init__(self, x, y)
          2 def __init__(self, x, y):
    ----> 3     self.x = x
          4     self.y = y
    

    Input In [25], in Test.__setattr__(self, name, value)
         10 if name == 'x':
         11     value = abs(value)
    ---> 12 setattr(self, name, value)
    

    Input In [25], in Test.__setattr__(self, name, value)
         10 if name == 'x':
         11     value = abs(value)
    ---> 12 setattr(self, name, value)
    

        [... skipping similar frames: Test.__setattr__ at line 12 (1482 times)]
    

    Input In [25], in Test.__setattr__(self, name, value)
         10 if name == 'x':
         11     value = abs(value)
    ---> 12 setattr(self, name, value)
    

    Input In [25], in Test.__setattr__(self, name, value)
          9 def __setattr__(self, name, value):
    ---> 10     if name == 'x':
         11         value = abs(value)
         12     setattr(self, name, value)
    

    RecursionError: maximum recursion depth exceeded in comparison


- 이를 해결하기 위해서는 setattr 의 기존 정의를 불러올 필요가 있다
- 한편, class 는 상속하지 않더라도 항상 object 라는 class 를 상속하고 있다


```python
class Test:
    def __init__(self):
        print(issubclass(Test, object))
        print(super().__dir__())  # 부모 클래스의 함수 목록을 반환한다

A = Test()
```

    True
    ['__module__', '__init__', '__dict__', '__weakref__', '__doc__', '__new__', '__repr__', '__hash__', '__str__', '__getattribute__', '__setattr__', '__delattr__', '__lt__', '__le__', '__eq__', '__ne__', '__gt__', '__ge__', '__reduce_ex__', '__reduce__', '__subclasshook__', '__init_subclass__', '__format__', '__sizeof__', '__dir__', '__class__']
    

- 그러므로 기존의 setattr 정의는 object 라는 부모 클래스에서 온 것이기에 <br>
  부모 클래스의 setattr 를 사용하게 되면 RecursionError 를 해결할 수 있다


```python
class Test:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return "({}, {})".format(self.x, self.y)
   
    def __setattr__(self, name, value):
        if name == 'x':
            value = abs(value)
        super().__setattr__(name, value)

A = Test(-1, 2)
print(A)
```

    (1, 2)
    
