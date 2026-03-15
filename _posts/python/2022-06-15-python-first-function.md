---
title: Python - 일급 함수(First Function/First Class)
date: 2022-06-15 16:22:20 +/-09:00
category: [python/basic]
tag: [python, first function, first class, 일급 함수]
---

- [**일급 함수**](#일급-함수)
- [**재귀 함수**](#재귀-함수)

# **일급 함수**

**함수임에도 변수와 동일하게 취급되는 함수를 말한다.**

일급 함수의 특징은 **재귀함수(Recursion Function)**를 만든 뒤 빈 클래스와의 교집합을 빼보면 알 수 있다.

```python
def minus(num):
  num -= 1
  if num == 0:
  return print("Zero!")
  print(num)
  return minus(num)

class N:
  pass
print(set(sorted(dir(minus)))) - set(set(sorted(dir(N))))
```

코드의 작동 방식은 나중에 생각하도록 하고, 일단 저 문장을 프린트해보면 집합들에 들은 매직 메소드들이 나온다.

```text
'\_\_defaults\_\_', '\_\_call\_\_', '\_\_annotations\_\_', '\_\_globals\_\_', '\_\_code\_\_', '\_\_get\_\_', '\_\_name\_\_', '\_\_builtins\_\_', '\_\_closure\_\_', '\_\_qualname\_\_', '\_\_kwdefaults\_\_'
```
여기에 나온 매직 메소드들을 사용할 수 있는 녀석이 바로 일급 함수다.

---

# **재귀 함수**

자기 자신을 반복해서 호출하는 함수를 말한다.

위의 프린트문은 지우고, minus에 들어갈 num값을 우리가 임의로 설정해준다.
```python
def minus(num):
  num -= 1
  if num == 0:
  return print("Zero!")
  print(num)
  return minus(num)

minus(int(input("type the number")))
```
num 값은 우리가 입력한 수가 된다. if 값이 0이 된다면 우리는 Zero 출력을 반환한다. 그렇지 않다면 minus 함수 자신을 계속해서 반환할 것이다.



다른 코드를 보자. 이 코드는 정말로 이해하기가 쉽다.
```python
def myList():

  return [i for i in range(1, 4)]

print(myList())
```
이 코드는 [1, 2, 3] 이라는 리스트를 반환할 것이 분명하다. 그렇다면,
```python
for i in myList():

  print(i)
```
혹시 이것도 가능하지 않을까? 정답은 뭘까?

답은 가능하다. 정말로 1과 2와 3을 순서대로 출력한다. 즉, 이것은 일급 함수라는 것이다.

---