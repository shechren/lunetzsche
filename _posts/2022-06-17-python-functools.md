---
title: Python - functools
date: 2022-06-17 21:06:41 +/-09:00
category: [python/advanced]
tag: [python, functools]
---

functools은 **고차 함수**를 위한 모듈이다. 다른 함수에 작용하거나 다른 함수를 반환하는 callable 객체에 대해 작용한다.

**지금은 본인이 아직 학습 중이기에 대표적인 것만 몇 개 꺼내서 써보도록 하자.**

**아주 간단한 예시만 할 것이다.**

```python
import functools as ft
```

#### **functools.reduce(함수,iter\[, 초기값\])**

초깃값을 받은 뒤 함수를 누적시켜 계산한다. 이해하기 쉬운 예시는 아래와 같다.

```python
#reduce(함수, iter, [생성자]
a = ft.reduce(lambda x, y: x+y, [i for i in range(1, 6)], 0)
print(a, "= ((((1 + 2) + 3) + 4) + 5)")

# 결과: 15 = ((((1 + 2) + 3) + 4) + 5)
```

반복 가능한 iter인 \[1, 2, 3, 4, 5\]를 받고, x에 대입된 1과 y에 대입된 2를 더한다. 이후 x(3)과 y(3)을 더한다. 이후 x(6)과 y(4)를 더하고 마지막으로 x(10)과 y(5)를 더한다.

#### **functool.partial(함수, \*args, \*\*keywords)**

하나의 인자만 받고 나머지 인자들은 고정한 함수를 만들 때 쓰인다.

```python
def addnum(a, b):
    return a + b

part = ft.partial(addnum, b=2)
print(addnum(2, 3))
print(part(3))
```

a + b를 해주는데 b의 값을 고정하여 a의 값만 넘기면 자동으로 계산이 되는 함수다.

```python
#2진법을 10진법으로 표기
binary = ft.partial(int, base=2)
print(binary.__doc__)
print(binary('1000'))

# 결과: 8
```

이런 식으로도 쓰인다.

### **functools.wraps(wrapped,assigned=WRAPPER_ASSIGNMENTS,updated=WRAPPER_UPDATES)**

데코레이터에 대한 이야기다. 데코레이터를 참조하자.

파이썬 공식 라이브러리에서는 다음과 같은 예시를 제공한다.

```python
>>> from functools import wraps
>>> def my_decorator(f):
...     @wraps(f)
...     def wrapper(*args, **kwds):
...         print('Calling decorated function')
...         return f(*args, **kwds)
...     return wrapper
...
>>> @my_decorator
... def example():
...     """Docstring"""
...     print('Called example function')
...
>>> example()
Calling decorated function
Called example function
>>> example.__name__
'example'
>>> example.__doc__
'Docstring'
```
  
---