```python
import sys

A = 12; B = 12.12
print(A, " ( byte :", sys.getsizeof(A), ")")
print(B, " ( byte :", sys.getsizeof(B), ")")

A = 12**50; B = (1.2)**50
print(A, " ( byte :", sys.getsizeof(A), ")")
print(B, " ( byte :", sys.getsizeof(B), ")")
```

    12  ( byte : 28 )
    12.12  ( byte : 24 )
    910043815000214977332758527534256632492715260325658624  ( byte : 48 )
    9100.438150002134  ( byte : 24 )
    
