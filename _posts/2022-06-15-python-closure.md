---
title: Python - 클로저(Closure)
date: 2022-06-15 16:43:14 +/-09:00
category: [python/advanced]
tag: [python, closure]
---

전역 변수는 말 그대로 전체 영역(Global Area)에 저장되어 있는 변수다.

지역 변수는 함수 내(Local Area)에 저장되어 있는 변수다.
```python
def func(a):
    print(a)
    print(b)
    b = 10
    
func(10)
```
다짜고짜 코드부터 보자. 어떤 결과가 일어날까?

a에는 func() 에서 10이 대입되었다. 따라서 a는 10이다.

문제는 b다. 절차지향 프로그래밍에 따라 print(b)를 먼저 실행한다. 따라서 에러가 난다.

b를 1열만 올려줘도 일어나지 않는 에러다.
```python
b = 10
def func(a):
    print(a)
    print(b)

func(10)
```
이렇게 하면 어떨까?

당연한 얘기겠지만 결과는 같다. 단, b는 로컬 변수가 아닌 전역 변수다.
```python
b = 20
def func(a):
    global b
    print(a)
    print(b)
    b = 30
func(10)
func(20)
```
이제 진짜 심각한 문제다. global b로 b를 전역 변수로 선언해주었다. 이제 b = 30은 전역변수다.

func() 를 2차례 실행했다. 어떤 결과가 나올까?

![python-closure-1.png](/assets/postingImage/python-closure-1.png)

첫 번째 함수다. a에서 10을 받았으니 10이 나오고, 전역 변수 b에서 20을 받았으니 20이 나온다.

![python-closure-2.png](/assets/postingImage/python-closure-2.png)

첫 함수가 끝나기 직전 b가 전역 변수로 선언되었다. 이제 2번째 func()로 간다.

a는 20이 선언되었고, b는 30이 선언되었다.

---

a와 b가 숙제를 시작했는데 a는 10시간이 걸리고 b는 5시간이 걸린다. 그런데 a가 5시간 먼저 숙제를 시작했다. 그럼 동시에 끝나야 하는데 a가 중간에 멈춘다. 이후 b도 멈춘다. 그러다 갑자기 숙제를 할 자리가 1개로 줄어들어서 둘 중 한 명만 할 수 있어서 돌아가면서 숙제를 한다. 이 경우 어떻게 할 것인가.

클로저는 서로 공유하지만 **변경되지 않는** 구조다. 클로저는 멀티스레드 프로그래밍에 강점을 가지고 있다. 또한 우리가 한 일(**불변 상태**)을 기억하고 있기에 책갈피를 꽃아두면 그 부분에서 다시 공부를 시작할 수 있는 것이다.

어떤 프로그래밍 언어건 함수는 일정 시간이 지나면 자신이 한 일에 대해 잊어버린다.

하지만 클로저는 그렇지 않다. 아래 코드는 클로저가 하는 일에 대해 대략적으로 설명할 수 있다.

```python
class Addition():
    def __init__(self):
        self.addition = []

    def __call__(self, m): #call 메소드로 클래스를 함수처럼 사용할 수 있다!
        self.addition.append(m)
        print(f"더해진 값 : {sum(self.addition)}, 횟수 : {len(self.addition)}")
        return sum(self.addition, len(self.addition))

a = Addition()
print(a(1))
print(a(2))
print(a(5))
```
결과부터 말하자면

**더해진 값 : 1, 횟수 : 1**  
**2**  
**더해진 값 : 3, 횟수 : 2**  
**5**  
**더해진 값 : 8, 횟수 : 3**  
**11**

이다. 놀랍게도 자신이 더한 값에 횟수를 누적해서 계속해서 반복하고 있다. 상술한, 자기가 공부한 범위를 놓치지 않고 계속해서 누적시켜 더하고 있는 것이다.

![python-closure-3.png](/assets/postingImage/python-closure-3.png)

이것을 이용하면 멀티스레딩을 비롯 게임에서의 세이브 등 여러가지를 할 수 있다.

이걸 클래스 내부에서만 할 수 있느냐? 그건 절대 아니다. 함수로도 구현이 가능하다.
```python
def myClosure():
    myList = [] # 지역 변수
    def addition(m):
        myList.append(m)
        return myList
    return addition

a = myClosure()
print(a(10))
print(a(20))
print(a(30))
print(a.__closure__[0].cell_contents)
```
우리는 리스트에 10, 20, 30을 더했다. 그리고 마지막으로 전체를 출력했다.

**\[10\]**  
**\[10, 20\]**  
**\[10, 20, 30\]**  
**\[10, 20, 30\]**

클로저는 우리가 한 일을 기억하고 있고, 계속해서 그 위에서 일을 하고 있다.

중요한 것은 return addition. 즉 함수 자체를 리턴하는 것이다.

---