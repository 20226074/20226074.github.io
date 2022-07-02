---
layout: single
title: "Data Type (complete)"
categories: coding
tags: [python, basic programming]
---

## Data Type
 - Numeric Data
 - Sequence Data
 - Mapping Data
 - Set Data
 - Boolean Data

<br> <br> <br> <br>

# Numeric Data
### Type : Integer(int), Float(float), Complex

<hr>

#### int 의 기본 크기는 28 byte 이다
- 24 byte 는 class int 를 형성하는 데 쓰인다
- 4 byte 는 실제로 입력되는 값의 저장에 쓰이며, 부족할 경우 4 byte 씩 크기가 증가한다

int 의 경우, 1 bit 는 $0(+)$ $or$ $1(-)$ 을 나타내고, 나머지 bit 는 $2^0, 2^1, 2^2, ...$ 의 포함 여부를 나타내어 정수를 표현한다


```python
import sys

print("{0} : {1}".format("bytes of 2"        .ljust(18), sys.getsizeof(2)))
print("{0} : {1}".format("bytes of 2**30 - 1".ljust(18), sys.getsizeof(2**30 - 1)))
print("{0} : {1}".format("bytes of 2**30"    .ljust(18), sys.getsizeof(2**30)))
print("{0} : {1}".format("bytes of 2**60 - 1".ljust(18), sys.getsizeof(2**60 - 1)))
print("{0} : {1}".format("bytes of 2**60"    .ljust(18), sys.getsizeof(2**60)))
```

    bytes of 2         : 28
    bytes of 2**30 - 1 : 28
    bytes of 2**30     : 32
    bytes of 2**60 - 1 : 32
    bytes of 2**60     : 36
    

<hr>

#### float 의 크기는 24 byte 이다
- 16 byte 는 class float 를 형성하는 데 쓰인다
- 8 byte 는 실제로 입력되는 값의 저장에 쓰이며, 정확하게 표현하지 못하더라도 크기를 증가시키지 않는다

float 의 경우, 1 bit 는 $0(+)$ $or$ $1(-)$ 를 나타내고, 11 bit 는 지수부, 52 bit 는 가수부로 사용하여 부동 소수점 표현으로 나타낸다

### 부동 소수점 표현
(1) 입력받은 float 의 정수부와 소수부를 2진수로 나타낸다
- 정수부는 int 와 동일한 논리이고, 소수부는 $2^{-1}, 2^{-2}, ...$ 를 사용하여 근사화시킨다 <br>

(2) 그렇게 만들어진 2진수의 소수점을 이동시켜서 $1.xxx..._{(2)}$ $\times$ $2^E$ 형태로 만들어준다
- 위의 과정을 '정규화'라고 하며, 이 때 $E$ 는 지수부이고, $xxx...$ 는 가수부이다
- 지수부는 부호를 나타내기 위해 실제 지수에 $2^{10}$ 를 더해서 저장한다.
  그러면 $-$ 의 경우 맨 앞의 bit 는 $0$ 이 되고, $+$ 의 경우 맨 앞의 bit 는 $1$ 이 된다
- 가수부는 52 bit 이므로, 앞에서부터 52개의 2진수를 받으며, 부족할 경우 뒤에 $0$을 채운다 <br>


$111.1$ 의 경우, 부호부는 $0(+)$ 이고, 정수부와 소수부를 2진수로 나타내면 $1101111.0001100110011001100...$ 이다 <br>
소수점을 이동시키면 $1.1011110001100110011001100...$ $\times$ $2^6$ 으로 나타내진다 <br>
그러므로 지수부는 $6 + 2^{10}$ $=$ $10000110_{(2)}$ 이고, 가수부는 $1011110001100110011001100...$ (52자리) 이다


```python
print("{0} : {1}".format("expression of (22)**50"  .ljust(24), (22)**50))
print("{0} : {1}".format("expression of (2.2)**50" .ljust(24), (2.2)**50))
print("{0} : {1}".format("expression of (0.22)**50".ljust(24), (0.22)**50))
# e+17 : (front number) * 10**17
# e-33 : (front number) * 10**(-33)
```

    expression of (22)**50   : 13217035032142513618039644258500715884299760500166608934135475994624
    expression of (2.2)**50  : 1.3217035032142566e+17
    expression of (0.22)**50 : 1.3217035032142518e-33
    

위와 같이 float 는 소수부의 근사화와 가수부의 크기 제한이 있기에 부정확성이 생길 수 있다 <br>

등호 판단을 위해서는 math.isclose(a, b, rel_tol=1e-09, abs_tol=0.0) 를 사용할 수 있으며 <br>
정확한 계산 자체를 해결하려면 decimal 모듈의 Decimal 인스턴스를 사용하여 연산해야 한다 <br>
- **Warning** : Decimal 은 고정 소수점을 사용하여 정확하게 표현하는 것이다
  - 지수부 없이 정수부와 소수부를 그대로 저장하기에, 크기를 많이 차지한다
  - 이미 근사화된 값을 정확하게 변환시킬 수는 없으므로, float 가 아니라 string 을 받아야 한다


```python
import math
from decimal import Decimal

print("{0} : {1}".format("0.1 + 0.2 == 0.3"                                 .ljust(50), 0.1 + 0.2 == 0.3))
print("{0} : {1}".format("math.isclose(0.1 + 0.2, 0.3)"                     .ljust(50), math.isclose(0.1 + 0.2, 0.3)))
print("{0} : {1}".format("Decimal('0.1') + Decimal('0.2') == Decimal('0.3')".ljust(50), Decimal('0.1') + Decimal('0.2') == Decimal('0.3'))); print()

print("{0} : {1}".format("0.1 + 0.2"                             .ljust(50), 0.1 + 0.2))
print("{0} : {1}".format("float(Decimal(0.1) + Decimal(0.2))"    .ljust(50), float(Decimal(0.1) + Decimal(0.2))))
print("{0} : {1}".format("float(Decimal('0.1') + Decimal('0.2'))".ljust(50), float(Decimal('0.1') + Decimal('0.2'))))
```

    0.1 + 0.2 == 0.3                                   : False
    math.isclose(0.1 + 0.2, 0.3)                       : True
    Decimal('0.1') + Decimal('0.2') == Decimal('0.3')  : True
    
    0.1 + 0.2                                          : 0.30000000000000004
    float(Decimal(0.1) + Decimal(0.2))                 : 0.30000000000000004
    float(Decimal('0.1') + Decimal('0.2'))             : 0.3
    

<hr>

#### complex 의 크기는 32 byte 이다
- A = complex(a, b) 라고 정의하면 A.real = a 이고 A.imag = b 가 되는데, 이 A.real 과 A.imag 는 float 이다
- 따라서 16 byte 로 형성되는 class complex 에 class float 의 내용이 포함된다
- 그리고 8 byte 는 A.real, 나머지 8 byte 는 A.imag 의 값의 저장에 쓰인다


```python
A = complex(0.3, 0.4)
print("|{0} + {1}i| = {2}".format(A.real, A.imag, abs(A)))
```

    |0.3 + 0.4i| = 0.5
    

<br> <br> <br> <br>

# Sequence Data
### Type : str, list, tuple

<hr>

#### str 은 문자열을 받을 수 있는 character data 이자 sequence data 이다  ( indexing 이 가능하다 )
- 하지만, item assignment 은 불가능하다  ( tuple 과 비슷하게 index 로 수정이 불가능하다 )
  - slice 와 + operator 로 수정할 수는 있다


```python
Str = 'Hello World!'

print("{0} : {1}".format("str", Str))
print("{0} : {1}, {2}, {3}, {4}".format("str[0], str[1], str[-1], str[::-1]", Str[0], Str[1], Str[-1], Str[::-1]))
print("{0} : {1}".format("len(str)", len(Str))); print()

print(Str .ljust(20), " |")
print(Str.center(20), " |")
print(Str .rjust(20), " |"); print()

print(Str.lower())
print(Str.upper()); print()

print("{0} : {1}".format("+ operator", Str.lower() + Str.upper()))
print("{0} : {1}".format("* operator", Str * 3)); print()

print("{0} : {1}".format("Is 'o' in 'str'?", 'o' in Str))
print("{0} : {1}".format("Index of 'o' before blank (using 'find')", Str.find('o', 0, Str.find(' '))))
print("{0} : {1}".format("Index of 'o' after blank (using 'index')", Str.index('o', Str.index(' '), Str.index('!'))));
try:
    print("index of 'x' (using 'find')  : ", end = "")
    print(Str.find('x'))
    print("index of 'x' (using 'index') : ", end = "")
    print(Str.index('x'))
except ValueError:
    print("ValueError"); print()

number = '1234'; print("'{0}' is Number? : {1}".format(number, number.isdigit())); print()

print("{0} : {1}".format("All blank                     ", ' '.join(Str))); print()

print("{0} : {1}".format("split based on 'o'            ", Str.split('o'))); print()

print("{0} : {1}".format("strip 'str.ljust(50)'         ", Str.ljust(50).strip())); print()

print("{0} : {1}".format("fill '0' until len(str) = 20  ", Str.zfill(20))); print()

print("{0} : {1}".format("replace 'Hello' with 'Goodbye'", Str.replace('Hello', 'Goodbye'))); print()
```

    str : Hello World!
    str[0], str[1], str[-1], str[::-1] : H, e, !, !dlroW olleH
    len(str) : 12
    
    Hello World!          |
        Hello World!      |
            Hello World!  |
    
    hello world!
    HELLO WORLD!
    
    + operator : hello world!HELLO WORLD!
    * operator : Hello World!Hello World!Hello World!
    
    Is 'o' in 'str'? : True
    Index of 'o' before blank (using 'find') : 4
    Index of 'o' after blank (using 'index') : 7
    index of 'x' (using 'find')  : -1
    index of 'x' (using 'index') : ValueError
    
    '1234' is Number? : True
    
    All blank                      : H e l l o   W o r l d !
    
    split based on 'o'             : ['Hell', ' W', 'rld!']
    
    strip 'str.ljust(50)'          : Hello World!
    
    fill '0' until len(str) = 20   : 00000000Hello World!
    
    replace 'Hello' with 'Goodbye' : Goodbye World!
    
    

<hr>

### list 은 수정이 가능한 Array 이다
<br>

```python
List = [1, 'A', ['Hello', 'World', '!']]
print("{0} : {1}".format("List", List))
print("{0} : {1}, {2}".format("len(List), len(List[2])", len(List), len(List[2]))); print();

print("{0} : {1}".format("+ operator", List + List[2]))
print("{0} : {1}".format("* operator", List * 3)); print()


A = List.copy(); B = List.copy()
A.append([2, 3]); B[len(B):len(B)] = [[2, 3]]
print("{0} : {1}".format("List.append([2, 3])"                 .ljust(40), A))
print("{0} : {1}".format("List[len(List):len(List)] = [[2, 3]]".ljust(40), B)); print()

A = List.copy(); B = List.copy(); C = List.copy()
A.extend([2, 3]); B[len(B):len(B)] = [2, 3]; C = C + [2, 3]
print("{0} : {1}".format("List.extend([2, 3])"               .ljust(40), A))
print("{0} : {1}".format("List[len(List):len(List)] = [2, 3]".ljust(40), B))
print("{0} : {1}".format("List = List + [2, 3]"              .ljust(40), C)); print()

A = List.copy(); B = List.copy();
A.insert(1, [2, 3]); B[1:1] = [[2, 3]]
print("{0} : {1}".format("List.insert(1, [2, 3])".ljust(40), A))
print("{0} : {1}".format("List[1:1] = [[2, 3]]"  .ljust(40), B)); print()


A = List.copy(); B = List.copy();
A[1:1] = [2, 3]; B = B[:1] + [2, 3] + B[1:]
print("{0} : {1}".format("List[1:1] = [2, 3]"                 .ljust(40), A))
print("{0} : {1}".format("List = List[:1] + [2, 3] + List[1:]".ljust(40), B)); print()

A = List.copy()
A.extend([1, 'A', 1, 'A'])
print("{0}.index('A') : {1}".format(A, A.index('A')))
print("{0}.index('A', 3, 6) : {1}".format(A, A.index('A', 3, 6)))
try:
    print("{0}.index('Hello') : ".format(A), end = '')
    print(A.index('Hello'))
except ValueError:
    print("ValueError ( cannot search in the other dimension )")
print("{0}.count('A') : {1}".format(A, A.count('A'))); print()


print("  {0}.pop()".format(A))
print(": {1}  ( value : {0} )".format(str(A.pop()), A)); print()

print("  {0}.pop(0)".format(A))
print(": {1}  ( value : {0} )".format(str(A.pop(0)), A)); print()

print("  {0}.remove('A')".format(A))
print(": {1}  ( value : {0} )".format(A.remove('A'), A)); print()

print("  del List[1:3]".format(A)); del A[1:3]
print(": {1}  ( value : {0} )".format(None, A)); print()

print("  {0}.clear()".format(A))
print(": {1}  ( value : {0} )".format(A.clear(), A)); print(); print()


List = [1, [2, 3, 4], 5, 6, 7]
print("{0}.reverse() : ".format(List), end = '')
List.reverse(); print(List); print()


List = ['aaaaa', 'a', 'bbb', 'C', 'B', 'A', 'AAA']
print("{0}.sort() : ".format(List), end = '')
List.sort(); print(List)
# List.sort 또는 max(List), min(List) 는 List 가 모두 하나의 Data Type 으로 되어있어야 한다
```

    List : [1, 'A', ['Hello', 'World', '!']]
    len(List), len(List[2]) : 3, 3
    
    + operator : [1, 'A', ['Hello', 'World', '!'], 'Hello', 'World', '!']
    * operator : [1, 'A', ['Hello', 'World', '!'], 1, 'A', ['Hello', 'World', '!'], 1, 'A', ['Hello', 'World', '!']]
    
    List.append([2, 3])                      : [1, 'A', ['Hello', 'World', '!'], [2, 3]]
    List[len(List):len(List)] = [[2, 3]]     : [1, 'A', ['Hello', 'World', '!'], [2, 3]]
    
    List.extend([2, 3])                      : [1, 'A', ['Hello', 'World', '!'], 2, 3]
    List[len(List):len(List)] = [2, 3]       : [1, 'A', ['Hello', 'World', '!'], 2, 3]
    List = List + [2, 3]                     : [1, 'A', ['Hello', 'World', '!'], 2, 3]
    
    List.insert(1, [2, 3])                   : [1, [2, 3], 'A', ['Hello', 'World', '!']]
    List[1:1] = [[2, 3]]                     : [1, [2, 3], 'A', ['Hello', 'World', '!']]
    
    List[1:1] = [2, 3]                       : [1, 2, 3, 'A', ['Hello', 'World', '!']]
    List = List[:1] + [2, 3] + List[1:]      : [1, 2, 3, 'A', ['Hello', 'World', '!']]
    
    [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1, 'A'].index('A') : 1
    [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1, 'A'].index('A', 3, 6) : 4
    [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1, 'A'].index('Hello') : ValueError ( cannot search in the other dimension )
    [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1, 'A'].count('A') : 3
    
      [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1, 'A'].pop()
    : [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1]  ( value : A )
    
      [1, 'A', ['Hello', 'World', '!'], 1, 'A', 1].pop(0)
    : ['A', ['Hello', 'World', '!'], 1, 'A', 1]  ( value : 1 )
    
      ['A', ['Hello', 'World', '!'], 1, 'A', 1].remove('A')
    : [['Hello', 'World', '!'], 1, 'A', 1]  ( value : None )
    
      del List[1:3]
    : [['Hello', 'World', '!'], 1]  ( value : None )
    
      [['Hello', 'World', '!'], 1].clear()
    : []  ( value : None )
    
    
    [1, [2, 3, 4], 5, 6, 7].reverse() : [7, 6, 5, [2, 3, 4], 1]
    
    ['aaaaa', 'a', 'bbb', 'C', 'B', 'A', 'AAA'].sort() : ['A', 'AAA', 'B', 'C', 'a', 'aaaaa', 'bbb']
    

<hr>

### tuple 은 수정이 불가능한 Array 이다  ( index 에 직접 값을 변경하는 것은 불가능하다 )
<br>

```python
print("{0} : {1}".format("type(('a')) ", type(('a'))))
print("{0} : {1}".format("type(('a',))", type(('a',)))); print()

Tuple = ('a', 'b', 'c')
print("{0} : {1}".format("Tuple", Tuple))
print("{0} : {1}".format("+ operator", Tuple + (1, 2, 3)))
print("{0} : {1}".format("* operator", Tuple * 3)); print()

print("{0} : {1}".format("('A',) + Tuple[1:]", ('A',) + Tuple[1:])); print()
# tuple 은 위와 같이 수정해야 한다  ( Tuple[0] = 'A' 또는 del Tuple[0] 불가능 )

a = 1; b = 2; c = 3
a, b, c = c, b, a
print(a, b, c)
```

    type(('a'))  : <class 'str'>
    type(('a',)) : <class 'tuple'>
    
    Tuple : ('a', 'b', 'c')
    + operator : ('a', 'b', 'c', 1, 2, 3)
    * operator : ('a', 'b', 'c', 'a', 'b', 'c', 'a', 'b', 'c')
    
    ('A',) + Tuple[1:] : ('A', 'b', 'c')
    
    3 2 1
    

<br> <br> <br> <br>

# Mapping Data
### Type : dictionary
### dictionary 는 { key : value } mapping 으로 이루어진 Array 이다
<br>

```python
Dict = {'a': 1, 'b': 2, 'c':'Hello'}


print("{0}\n: {1}\n  {2}, {3}".format("Dict.items(), Dict.keys(), Dict.values()", Dict.items(), Dict.keys(), Dict.values()))
print(Dict.items() == Dict.items(), Dict.keys() == Dict.keys(), Dict.values() == Dict.values(), Dict.values().__eq__(Dict.values())); print()
# Dict.keys() 는 entry 가 unique, hashable 하므로 set-like 하지만, Dict.values() 는 not set-like 하므로, '==' 이 구현되어 있지 않다
# 한편, Dict.keys() 는 set-like 하므로 순서가 없다  ( 항상 Dict.keys() 가 그 순서대로 나온다는 보장은 없다 )

print(list(Dict.keys()), list(Dict), list(Dict.items())); print()
# Dict.items(), Dict.keys(), Dict.values() 는 dictview object 이다
# Dictionary 를 iterable 로 사용하면 keys 를 가져온다

# Dictionary 는 + operator 와 * operator 를 사용할 수 없다


print("{0} : {1}, {2}".format("Dict.get('a'), Dict['a']", Dict['a'], Dict.get('a')))
try:
    print("Dict.get('d'), Dict['d'] : ", end = '')
    print(Dict.get('d'), end = ', ')
    print(Dict['d'])
except KeyError:
    print("KeyError"); print()


Dict['a'] = 'Hello'
print("{0} : {1}".format("Dict['a'] = 'Hello'"              .ljust(35), Dict))

Dict['d'] = 'World'
print("{0} : {1}".format("Dict['d'] = 'World'"              .ljust(35), Dict))

Dict.update({'c': 3, 'd': 4})
print("{0} : {1}".format("Dict.update({'c': 3, 'd' = 4})"   .ljust(35), Dict))

Dict.update([['a', 1], ['e', 5]])
print("{0} : {1}".format("Dict.update([['a', 1], ['e', 5]])".ljust(35), Dict))

del Dict['b']
print("{0} : {1}".format("del Dict['b']"                    .ljust(35), Dict))

output = Dict.pop('c')
print("{0} : {1}  ( output : {2} )".format("output = Dict.pop('c')" .ljust(35), Dict, output))

output = Dict.popitem()
print("{0} : {1}  ( output : {2} )".format("output = Dict.popitem()".ljust(35), Dict, output)); print()

print("{0} : {1}".format("dict.fromkeys(['A', 'B', 'C'], 'Ready')", dict.fromkeys(['A', 'B', 'C'], 'Ready')))
```

    Dict.items(), Dict.keys(), Dict.values()
    : dict_items([('a', 1), ('b', 2), ('c', 'Hello')])
      dict_keys(['a', 'b', 'c']), dict_values([1, 2, 'Hello'])
    True True False NotImplemented
    
    ['a', 'b', 'c'] ['a', 'b', 'c'] [('a', 1), ('b', 2), ('c', 'Hello')]
    
    Dict.get('a'), Dict['a'] : 1, 1
    Dict.get('d'), Dict['d'] : None, KeyError
    
    Dict['a'] = 'Hello'                 : {'a': 'Hello', 'b': 2, 'c': 'Hello'}
    Dict['d'] = 'World'                 : {'a': 'Hello', 'b': 2, 'c': 'Hello', 'd': 'World'}
    Dict.update({'c': 3, 'd' = 4})      : {'a': 'Hello', 'b': 2, 'c': 3, 'd': 4}
    Dict.update([['a', 1], ['e', 5]])   : {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
    del Dict['b']                       : {'a': 1, 'c': 3, 'd': 4, 'e': 5}
    output = Dict.pop('c')              : {'a': 1, 'd': 4, 'e': 5}  ( output : 3 )
    output = Dict.popitem()             : {'a': 1, 'd': 4}  ( output : ('e', 5) )
    
    dict.fromkeys(['A', 'B', 'C'], 'Ready') : {'A': 'Ready', 'B': 'Ready', 'C': 'Ready'}
    

<br> <br>
# Set Data
### Type : set
### set 은 순서가 없어 index 가 불가능한 Array 이다
<br>

```python
Set = {1, 2, 3, 4, 5}
print("{0} : {1}".format("Set"                                 .ljust(30), Set))

Set.add(6)
print("{0} : {1}".format("Set.add(6)"                          .ljust(30), Set))

Set.update([5, 6, 7, 8])
print("{0} : {1}".format("Set.update([5, 6, 7, 8])"            .ljust(30), Set))

Set.discard(8)
print("{0} : {1}".format("Set.discard(8)"                      .ljust(30), Set))

Set.remove(7)
print("{0} : {1}".format("Set.remove(7)"                       .ljust(30), Set))

output = Set.pop()
print("{0} : {1}  ( output = {2} )".format("output = Set.pop()".ljust(30), Set, output))

Set.discard(9)
print("{0} : {1}".format("Set.discard(9)"                      .ljust(30), Set))

try:
    print("Set.remove(10)".ljust(30), ": ", end = '')
    Set.remove(10)
except KeyError:
    print('KeyError'); print()

    
print("{0} : {1}".format("Set"                                   .ljust(40), Set))
print("{0} : {1}".format("Set < {1, 2, 3, 4, 5, 6}"              .ljust(40), Set < {1, 2, 3, 4, 5, 6}))
print("{0} : {1}".format("Set > {3, 4, 5, 6}"                    .ljust(40), Set > {3, 4, 5, 6}))
print("{0} : {1}".format("Set | {5, 6, 7, 8}"                    .ljust(40), Set | {5, 6, 7, 8} ))
print("{0} : {1}".format("Set.union({5, 6, 7, 8})"               .ljust(40), Set.union({5, 6, 7, 8})))
print("{0} : {1}".format("Set & {5, 6, 7, 8}"                    .ljust(40), Set & {5, 6, 7, 8} ))
print("{0} : {1}".format("Set.intersection({5, 6, 7, 8})"        .ljust(40), Set.intersection({5, 6, 7, 8})))
print("{0} : {1}".format("Set - {5, 6, 7, 8}"                    .ljust(40), Set - {5, 6, 7, 8} ))
print("{0} : {1}".format("Set.difference({5, 6, 7, 8})"          .ljust(40), Set.difference({5, 6, 7, 8})))
print("{0} : {1}".format("Set ^ {5, 6, 7, 8}"                    .ljust(40), Set ^ {5, 6, 7, 8} ))
print("{0} : {1}".format("Set.symmetric_difference({5, 6, 7, 8})".ljust(40), Set.symmetric_difference({5, 6, 7, 8})))
```

    Set                            : {1, 2, 3, 4, 5}
    Set.add(6)                     : {1, 2, 3, 4, 5, 6}
    Set.update([5, 6, 7, 8])       : {1, 2, 3, 4, 5, 6, 7, 8}
    Set.discard(8)                 : {1, 2, 3, 4, 5, 6, 7}
    Set.remove(7)                  : {1, 2, 3, 4, 5, 6}
    output = Set.pop()             : {2, 3, 4, 5, 6}  ( output = 1 )
    Set.discard(9)                 : {2, 3, 4, 5, 6}
    Set.remove(10)                 : KeyError
    
    Set                                      : {2, 3, 4, 5, 6}
    Set < {1, 2, 3, 4, 5, 6}                 : True
    Set > {3, 4, 5, 6}                       : True
    Set | {5, 6, 7, 8}                       : {2, 3, 4, 5, 6, 7, 8}
    Set.union({5, 6, 7, 8})                  : {2, 3, 4, 5, 6, 7, 8}
    Set & {5, 6, 7, 8}                       : {5, 6}
    Set.intersection({5, 6, 7, 8})           : {5, 6}
    Set - {5, 6, 7, 8}                       : {2, 3, 4}
    Set.difference({5, 6, 7, 8})             : {2, 3, 4}
    Set ^ {5, 6, 7, 8}                       : {2, 3, 4, 7, 8}
    Set.symmetric_difference({5, 6, 7, 8})   : {2, 3, 4, 7, 8}
    

<br> <br> <br> <br>

# Boolean Data
### Type : bool
### bool 은 True/False 만을 가지는 자료형이다
<br>


```python
print("{0} : {1}".format("bool(0)".ljust(15), bool(0)))
print("{0} : {1}".format("bool(None)".ljust(15), bool(None)))
print("{0} : {1}".format("bool(1)".ljust(15), bool(1)))
print("{0} : {1}".format("bool(10000000)".ljust(15), bool(10000000)))
print("{0} : {1}".format("bool('asdf')".ljust(15), bool('asdf')))
```

    bool(0)         : False
    bool(None)      : False
    bool(1)         : True
    bool(10000000)  : True
    bool('asdf')    : True
    
