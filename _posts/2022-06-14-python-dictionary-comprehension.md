---
title: Python - Dictionary Comprehension
date: 2022-06-14 13:58:32 +/-09:00
category: [python/basic]
tag: [python, dictionary comprehension, 지능형 딕셔너리]
---

**지능형 딕셔너리**라고도 하지만 여기서는 딕셔너리 컴프리헨션이라고 부를 것이다. 이 글을 읽기 전에 리스트 컴프리헨션을 먼저 읽어두자.


[List Comprehension](https://shechren.github.io/lunetzsche/posts/python-list-comprehension/)

---

우리는 카드 게임을 만들고 싶다. 그걸 위해서 우선 리스트 데이터가 필요하다.
```python
name = ["Knight", "Archer", "Priest"]
att = [10, 15, 5]
hp = [20, 15, 20]
```

됐다. 이제 각 이름의 각각의 정수를 대입하여 딕셔너리로 만들면 된다. 그런데 저렇게 단순히 3개만 있으면 그냥 노가다로 Dict를 만들겠지만, 만약 카드 게임이 너무 발전해서 카드가 엄청나게 늘어난다면 그렇게까지 하다간 머리가 터질 것이다. 물론 그렇게 되면 csv 파일이건 sql로 하건 다른 데이터 방식이 있겠지만... 하여튼 우리는 이걸 딕셔너리 컴프리헨션을 통해 다루고 싶다. 왜? 그게 편하니까 말이다.



일단 그 이전에 우리는 for문으로 이것을 만들어야 한다. 우리는 이미 리스트 컴프리헨션을 통해 대략적으로 방법은 알고 있을 것이다. 다만 세부적인 방법에서 약간 다르다.
```python
CardsDict = {}
for a, b, c in zip(name, att, hp):
  CardsDict[a] = b, c
print(CardsDict)
```
3열의 CardsDict[?] 에서 ?에 들어가는 녀석이 바로 key다. 우리는 name을 a에 넘겨 key로 쓸 것이기 때문에 a를 넣어주고, 그 Values에 b와 c를 넣어준다.

**결과: {'Knight': (10, 20), 'Archer': (15, 15), 'Priest': (5, 20)}**

---

그런데 조금 길어 맘에 안 든다. 이것을 딕셔너리 컴프리헨션으로 만들어보자. 방법은 같다. 단지 keys:values 를 앞에 적으면 되는 것이다.

```python
CardsDict = {a: (b, c) for a, b, c in zip(name, att, hp)}
print(CardsDict)
```
b와 c를 소괄호로 묶어두지 않으면 오류가 난다. 반드시 밸류가 여럿이면 밸류를 묶어서 저장해주자.

당연한 얘기겠지만 중복 Dict도 문제없다.

```python
CardsDict = {a: {b, c} for a, b, c in zip(name, att, hp)}
print(CardsDict)
```

---