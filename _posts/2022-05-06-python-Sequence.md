---
title: Python - 시퀀스(sequence)
date: 2022-05-06 19:45:10 +/-09:00
category: [python/basic]
tag: [python, sequence]
---

**컨테이너 시퀀스**는 서로 다른 자료형을 담을 수 있으며 list, tuple. collecction.deque형이 이에 속한다.
**균일 시퀀스**는 단 하나의 자료형만 담을 수 있으며 str, bytes, bytearray, memoryview, array.array형이 이에 속한다.

시퀀스형은 가변형에 따라 분류할 수도 있다.
가변 시퀀스에는 list, bytearray, array.array, collections.deque, memoryview형이 이에 속한다.
불변 시퀀스에는 tuple, str, bytes형이 이에 속한다.

우리는 지난 시간에 namedtuple을 이용한 플레잉카드 덱을 만드는 일을 해봤을 것이다.
하지만 지능형 리스트는 구문이 짧아야 가독성이 좋기 때문에 언제 어디서 사용할지 잘 결정해야 한다.
지능형 리스트를 잘 이용한다면 **데카르트 곱**을 구현할 수 있다. 데카르트 곱은 각 집합에 있는 모든 가능성을 지닌 새로운 집합이라고 생각하면 쉽다. 예를 들어 플레잉카드에서 숫자가 아닌 카드들의 데카르트 곱을 만든다고 하면,
["Jade", "Queen", "King", "Ace"]라는 집합과 ["Clubs", "Diamonds", "Spades", "Hearts"]를 곱하면 될 것이다.
그런데 실제로 이 리스트들끼리 곱해 실행하면 에러가 난다.
```python
a = ["Jade", "Queen", "King", "Ace"]
b = ["Clubs", "Diamonds", "Spades", "Hearts"]
cList = a \* b
print(cList)
```

```text
cList = a \* b 
TypeError: can't multiply sequence by non-int of type 'list'
```
리스트는 곱할 수 없다고 바로 알려주게 된다. 어? 그럼 어떻게 하지?
구구단을 만들 때 우리는 어떻게 만들더라? 바로 그렇다 이중 for문이다.
이중 for문을 이용해 우리는 결과값을 뽑아낼 수 있다.
```python
aList = ["Clubs", "Diamonds", "Spades", "Hearts"]
bList = ["Jade", "Queen", "King", "Ace"]
for a in aList:
  for b in bList:
    print(a, b)
```
총 16개의 카드 데이터가 뽑힌다.

---