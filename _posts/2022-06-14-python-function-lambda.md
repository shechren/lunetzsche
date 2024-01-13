---
title: Python - 함수(Function)와 람다식(Lambda)
date: 2022-06-14 17:19:37 +/-09:00
category: [python/basic]
tag: [python, function, def, lambda]
---

함수란 우리가 원하는 명령을 만들어 그것을 저장하고, 원하는 때에 꺼내서 쓰는 모종의 것을 의미한다. 이미 우리는 수많은 내장함수들을 알고 있다. len, range, map, filter 등...

함수를 만들 적에는 대개 이런 식으로 만들 것이다.
```python
def addition(a, b):
  return print(f"둘을 더한 값은 {a + b}입니다.")
a = int(input("1번 정수를 입력해주세요."))
b = int(input("2번 정수를 입력해주세요."))
addition(a, b)
```

def addtion은 a와 b를 **인자(Parameters)**로 받아 return을 해준다. 그리고 addition을 실행시킬 때는 a와 b라는 **인자(Arguments)**를 넘긴다.



이해가 쉽게 안 될 것이다.

일단 저 위의 함수에서는 단 하나의 내용밖에 없다. a + b의 값을 보여주는 것이다.

그리고 우리는 절차지향 프로그래밍에 따라 a input을 실행하고, b 인풋을 실행한다. 그리고 그 값을 addtion()에 넘기면 def addition이 실행되며 값을 받고, 마지막으로 return이 실행되는 것이다.



매커니즘을 그림으로 표현하면 이하와 같은데, 사실 설명이랄 것도 없다.


![python-function-lambda.png](/assets/postingImage/python-function-lambda.png)

---


람다는 이를 더 간단하게 한다. **익명함수**라고 쓰며, 간단한 함수가 필요한데 하나 쓰고 버리기엔 아까우니까 아예 문장 자체에 함수를 넣어버리는 것이다.


```python
a = int(input("변수 a를 입력해주세요."))
b = int(input("변수 b를 입력해주세요."))
c = lambda a, b: a + b
print(c(a, b))
```

c는 람다식이다. 먼저 람다를 쓰고, 받을 일회용 함수들을 넣는다. 이후 : 뒤에 계산식을 쓴다.

이 람다 함수에는 한 번 쓰고 버리는 a와 b가 들어갔다. 앞에 들어간 a와 b가 그것이다. 그리고 우리는 1열에서 받아온 a와 2열에서 받아온 b를 각각 더해서 c에 값을 대입하고, 4열에서는 람다함수 c를 출력하는데 여기서 a값과 b값을 넘겨줘야 한다.



아 그리고 람다 함수는 한 번 쓰고 버리는 일회용 함수가 들어가니까 **map** 또는 **filter** 함수에서 굉장히 유용하게 사용된다.

---