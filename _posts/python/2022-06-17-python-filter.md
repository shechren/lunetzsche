---
title: Python - filter
date: 2022-06-17 13:49:50 +/-09:00
category: [python/basic]
tag: [python, filter, lambda]
---

여기서는 제네레이터 함수 filter에 대해 설명할 것이다.

```python
Test = [25, 78, 910, 3060, 6750]

def divide(num):
  if num % 25 == 0:
    return True
    else:
      return False

newList = []
for i in Test:
  if divide(i):
    newList.append(i)
print(newList)
```

위는 Test 안의 수가 25로 나누어 떨어지는지 계산하고 이후 나눠지는 숫자를 출력하는 코드다.

고작 이거 하나 하자고 저 긴 코드를 쓰기에는 좀... 그렇다.

filter 함수는 조건문을 통과한 녀석들을 **iterator로 만들어서 반환**해준다.

그런데 map과는 다르게, 이 함수는 True인지 False인지를 먼저 가린다. 즉, True인 값만 반환해준다. 그 방식이 바로 위와 같은 것이다.

filter 함수를 쓰는 방법은 다음과 같다.

filter(함수, iter)

첫 번째에 들어갈 함수를 적는다. 위에서는 divide다. 두 번째에 iter 객체를 적는다. 위에서 Test다. 곧바로 실행해보자.

```python
a = filter(divide, Test)
print(list(a))
```

정상적으로 25와 6750이 출력되는 것을 알 수 있다.

그런데 이렇게 써도 뭔가 찝찝하다. divide 함수가 단 한 번 사용되고 버려지는 것이 아쉽다.

여기서 lambda 함수로 divide 함수를 바꿔보자.

```python
b = filter(lambda x: x % 25 == 0, Test)
print(list(b))
```

더 간단해졌다. 이렇게 쓰는 것이 필터의 정석과도 같다.

---