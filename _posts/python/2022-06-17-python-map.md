---
title: Python - Map
date: 2022-06-17 13:31:36 +/-09:00
category: [python/basic]
tag: [python, map, lambda]
---

여기서는 제네레이터 함수 중 map에 대해 설명한다.
```python
Test = [1, 2, 3] # this List is iterable

def sq(num):
    return num * num

for i in Test:
    print(sq(i))
```

sq는 인자값의 제곱을 반환한다.  
즉, 우리는 1, 2, 3의 제곱을 반환하는 함수를 만든 것이다.  
**1**  
**4**  
**9**  
   
map 함수는 각 iter의 요소를 일일히 전부 처리해준다. 예를 들어 리스트마다 전부 1을 더하고 싶다면 map함수를 통해서 쉽게 처리할 수 있다.  
map 함수의 사용법은 다음과 같다. map(함수, iter)  
첫 번째 인자값으로 함수를 받는다. 두 번째 인자값으로 iter를 받는다. 함수의 리턴값이 num + 1이라면 1 + 1, 2 + 1, 3 + 1을 전부 해준다.  
같은 맥락으로 위 코드도 바꿀 수 있다. 다만 그냥 출력 시 제네레이터 함수를 반환하므로 리스트로 변환해서 처리해보자.

```python
a = map(sq, Test)
print(list(a))
```

**\[1, 4, 9\]**  
그런데 겨우 이거 하나 하자고 함수를 만들기는 좀 아깝다. 다시 쓸 곳이 있는 함수라면 모르지만 이건 일회용 함수다. 이건 역시 람다식을 하면 좋을 것이다.

```python
b = map(lambda x: x * x, Test)
print(list(b))
```  

---