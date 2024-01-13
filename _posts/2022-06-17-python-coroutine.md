---
title: Python - Coroutine
date: 2022-06-17 17:14:19 +/-09:00
category: [python/asynchronous]
tag: [python, coroutine]
---

코루틴은 제네레이터에서 진화한 구조로 2006년(Python 2.5)에서 구현되었다. 이후로도 다양한 메소드가 추가되며 Python 3.3에서는 yield from도 추가되는 등 지금도 계속 발전하고 있다.  
  
미리 설명해두지만 코루틴을 공부하기 전에 **일급 함수, 클로저, 제네레이터, 데코레이터를 반드시 선행 공부해야 한다.**  
  
무거운 역사 얘기는 집어치우고 싶지만, 코루틴은 그만큼 복잡하고 파이썬 싱글 스레드 비동기 멀티태스킹의 총결산이라고 봐도 될만큼 아름다운 구조다.  
  
이 아름다운 구조를 단 하나의 코드에 요약해보자.

```python
def cou():
    print("start cou")
    a = yield
    print(f"cou received {a}")

    #서브 루틴
#메인 루틴
c = cou()
print(next(c))
c.send(1)
```
코루틴은 메인 루틴과 서브 루틴 사이에서 서로 정보를 주고받을 수 있다. 바로 저 send가 그 역할을 해주는 것이다.

![python-coroutine](/assets/postingImage/python-coroutine.png)

너무 아름다운 구조답게 첫 코드부터 StopIteration 에러를 내뱉는다.  
우리는 변수 c를 제네레이터 함수 cou()로 선언했다. 여기서 **일급 함수와 제네레이터를 사전에 배워야 한다는 것을 알 수 있다.**  
print(next(c))는 제네레이터 함수 cou()를 출력하기 시작할 것이다.  
start cou를 출력하고  
a의 타입은 None이다. 그대로 반환한다.  
c.send(1)은 a의 값에 1을 부여한다. 즉, 우리는 8열 c에서 cou() 내에 있는 a에 값을 보낸 것이다. 메인 루틴에서 서브 루틴으로 보낸 것과 같다.  
마지막으로 4열을 뱉은 뒤 더이상 반복할 수 없으니 StopIteration으로 마친다.  
  
일반적인 제네레이터 함수랑은 뭐가 다른지 다들 눈치챘을 것이다. yield 메소드 좌측에 a = 가 생긴 것이다.  
곧 그것이 코루틴이라고 해도 좋을 것이다.

---
```python
def cou2(i):
    print(f"start cou2, {i}")
    j = yield i
    print(f"cou received {j}")
    k = yield i + j
    print(f"cou received {k}")

c2 = cou2(10)
```
새로운 예시를 볼 시간이다. 아까보다 조금 더 복잡해졌다. 하지만 쉽게 이해하는 방법이 있다.  
변수는 오른쪽에서 왼쪽에 값을 넣는다. 즉 왼쪽에 있는 녀석은 값을 **받는** 녀석이고, 오른쪽에 있는 녀석은 값을 **보내는** 녀석이라고 보면 된다. 그냥 yield 자체를 return과 비슷하게 여겨도 상관 없을 것이다.  
이번에는 코루틴 자체를 분석하기 위해 코드 하나를 추가한다.

```python
from inspect import getgeneratorstate as gg
```

그리고 프린트해보며 값을 분석해보자.

```python
print(gg(c2))
print(next(c2))
print(gg(c2))
c2.send(20)
print(gg(c2))
print(next(c2))
```

**GEN_CREATED**  
**start cou2, 10**  
**10**  
**GEN_SUSPENDED**  
**cou received 20**  
**GEN_SUSPENDED**  
**cou received None**  
아까의 그것을 이해했다면 이해가 쉬울 것이다. next(c2)에 처음부터 10이라는 값이 들어가 있으니 10을 출력하고, c.send(20)에서 20을 보내니 cou received 20 이 나온다. 마지막으로 None을 반환한 뒤 StopIteration으로 마친다.  
그런데 중간에 있는 대문자의 저것은 대체 뭘까 생각이 들 수도 있다. getgeneratorstate로 추가한 것인데, 설명하자면 다음과 같다.  
"GEN_CREATED" : 실행을 시작하기 위해 대기하고 있는 상태  
"GEN_RUNNING" : 현재 인터프리터가 실행하고 있는 상태  
"GEN_SUSPENDED" : 현재 yield 문에서 대기하고 있는 상태  
"GEN_CLOSED" : 실행이 완료된 상태  
  
즉, 저것은 우리가 실행한 코루틴이 현재 어느 상태에 있는지 출력해주는 장치와 같은 것이다. 이걸 보고 당신이 곧바로 **데코레이터**를 떠올렸다면 당신은 파이썬 프로그래밍을 제대로 공부한 것이다. 물론 데코레이터와 완전히 같은 구조는 아니지만, 우리는 앞으로도 위와 같은 장치들을 데코레이터로 만들 테니까.

---

그런데 상술했듯이 우리가 만든 코루틴들, 죄다 StopIteration에 걸려 삐걱대고 있다. 어떻게 기동해야 할까?  
전문가를 위한 파이썬에서는 다음과 같이 나온다. _**코루틴은 기동되기 전에는 할 수 있는 일이 거의 없다. 코루틴을 편리하게 사용할 수 있도록 기동하는 다음과 같은 데코레이터가 종종 사용된다.**_

```python
from functools import wraps

def cou(func):
    @wraps(func)
    def primer(*args, **kwargs):
        gen = func(*args, **kwargs)
        next(gen)
        return(gen)
    return primer
```
#### **본 코드를 보면 일급 함수와 클로저, 제네레이터, 데코레이터까지 코루틴 이전에 배운 모든 구조가 쓰였다. 다시 말하지만 저것들을 선행 공부해야 코루틴을 사용할 수 있다.**

  
우리는 파이썬의 꽃이라고 볼 수 있는 코루틴을 기동시키는 코드를 만들었다. 이제 다시 전문가를 위한 파이썬에 나온 제네레이터 함수를 다시 보자.

```python
#이동 평균을 구하는 방법
def averager():
    total = 0.0 # float다.
    count = 0
    average = None
    #위를 두고 뭐라고 했지? 지역 변수(Local Function), 그리고 Instance Attribute다.
    while True:
        term = yield average
        total += term
        count += 1
        average = total/count
```

이제 위 두 개를 조합해서 코루틴을 완성할 차례다.

```python
from functools import wraps

def cou(func):
    @wraps(func)
    def primer(*args, **kwargs):
        gen = func(*args, **kwargs)
        next(gen)
        return gen
    return primer

@cou
def averager():
    total = 0.0
    count = 0
    average = None
    while True:
        term = yield average
        total += term
        count += 1
        average = total/count

coro_avg = averager()

print(coro_avg.send(10))
print(coro_avg.send(20))
print(coro_avg.send(30))
```

마지막 출력문을 출력해보고 평균값이 나오는지 계산해보자.  
  
추신: 코루틴을 배우니 코드가 너무 우아해서 공부하다가 감동의 눈물을 흘릴 뻔했다.

---