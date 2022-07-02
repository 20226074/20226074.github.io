# print example

<hr>
<br> <br>

### \ 의 사용법


```python
print('\''); print("\"");
print("\ 1") # print("\") 로 적으면 Backslash 연산으로 인해 SyntaxError 발생
print("\\1") # print("\\") 로 적어야 함
print("1234" + "\rA")  # \r : 커서를 맨 앞으로 이동시킴
print("1\t2\t3")
print("1234" + "\b") # \b : Backspace
```

    '
    "
    \ 1
    \1
    A
    1	2	3
    1234
    

<br>

### % 의 사용법


```python
print("%d, %f, %E, %s" % (1, 1.23, 123.45, "asdf"))
print("(%10d), (%-10d), (%5d)" % (1, 2, 3000000000))
print("(%-10f), (%.2f), (%10.4f), (%5.5f)" % (1.23, 3.4567, 1.23, 12345.6789))
print("(%E), (%10.3E), (%20.10E)" % (12345.6789, 12345.6789, 12345.6789))
print("(%20s), (%5s)" % ("asdfasdf", "asdfasdf"))
```

    1, 1.230000, 1.234500E+02, asdf
    (         1), (2         ), (3000000000)
    (1.230000  ), (3.46), (    1.2300), (12345.67890)
    (1.234568E+04), ( 1.235E+04), (    1.2345678900E+04)
    (            asdfasdf), (asdfasdf)
    1
    

<br>

### format 의 사용법


```python
print("{} {} {}".format("1", "2", "3"))
print("{2} {1} {0}".format("a", "b", "c"))
print("{x} {y} {0}".format('Z', y='Y', x='X'))
print("({0:10s}), ({1:10d})".format('asdf', 1234))
x = 1234.5678; y = 123.456
print("({x:10.3f}), ({y:10.3E})".format(x=x, y=y))
```

    1 2 3
    c b a
    X Y Z
    (asdf      ), (      1234)
    (  1234.568), ( 1.235E+02)
    

<br>

### f-String 의 사용법


```python
x = '1234'
print(f'AB {x} CD')
print(f'AB {int(x) + 1} CD')
print(f'AB {input("input : ")} CD')
List = [1, 2, 3]
print(f'AB {List[:2]} CD {x}')
print(f'AB {x if int(x) < 0 else int(x) * -1} CD')
```

    AB 1234 CD
    AB 1235 CD
    input : Check
    AB Check CD
    AB [1, 2] CD 1234
    AB -1234 CD
    
