---
title: Python - namedtuple로 덱(Deck) 만들기
date: 2022-05-03 21:26:03 +/-09:00
category: [python/basic]
tag: [python, namedtuple]
---
우리는 일반적으로 플레잉 카드가 들은 덱을 어떻게 만든다고 생각할까?
2부터 A까지 들은 리스트를 만들고, 곱하기 4를 하면 완성될 것이다.
방법 a를 통해 시도해보자.

```python
a = ["2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "K", "Q", "A"] \* 4

print(len(a))
```

하지만 이것은 해당 인덱스를 찾아서 뽑아와야 하기에 매우 번거로운 일이다. 게다가 카드의 종류는 뽑을 수 없다. 우리는 이것을 조금 더 직관적으로 바꿔야만 한다.
```python
bType = "C, D, S, H".split(",")
bNum = [str(i) for i in range(2, 15)]
print(bType)
print(bNum)
```
방법 b. 각각을 bType과 bNum으로 분리해서 출력한다. 어디서 어디까지? bType은 4개의 카드 타입을 가지고 있고, bNum은 2부터 14까지의 값을 저장한다. 왜 14까지이냐, 10은 J, 11은 Q, 12는 K, 13은 A이기 때문이다.
```text
['C', ' D', ' S', ' H'] 
['2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14']
```
출력 결과, 모든 것이 잘 뽑혔다. 하지만 리스트이기에 리스트끼리 곱할 수는 없다. 우리가 이것의 길이를 구하고 싶다면 타입을 int로 변경하거나 해야 한다.
하지만 이래도 뭔가 부족하다... 이것은 따지고 보면 네임드튜플이 아니다. 이제 우리는 네임드튜플을 제대로 활용할 시간이 온 것이다.

이하 네임드튜플을 선언하는 방식이다.
제일 먼저 필요 모듈을 import한다.
```python
from collections import namedtuple
```
네임드튜플 = namedtuple("네임드튜플", ["우리가 넣을 값"])
당연한 얘기겠지만 값이 2개인데 3개를 호출한다던가 하면 당연히 에러가 난다. 인덱스에 유의하자.
```python
NameA = namedtuple("NameA", ["cardType", "cardNum"])
```
네임드튜플을 선언했다. 이제 네임드튜플 안에서 빙글빙글 돌아 혼자서 덱의 모든 정보를 저장하는 resultA 변수를 만들자.
```python
resultA = [NameA(aType, aNum)

for aType in "C, D, S, H".split(",")
  for aNum in [str(n)
    for n in range(2, 15)]]
```
resultA의 NameA는 aType과 aNum을 갖고 있다. cardType과 cardNum의 인덱스와 일치한다.
Clubs, Diamonds, Spades, Hearts를 스플릿으로 나눠준다.
이후 for문을 반복하는데, 문자열 2부터 15까지 반복하기에 for문을 또 추가해준다.
결과값을 출력해보자.

```python
print(len(resultA))
print(resultA)
for a in resultA:
  print(a)
print(len(resultA))
```
: resultA의 길이를 반환한다. 당연하겠지만 52를 반환한다.
두 번째 프린트문은 1개의 행에 모든 카드를 반환한다.
NameA(cardType='C', cardNum='2'), NameA(cardType='C', cardNum='3'), NameA(cardType='C', cardNum='4') ... 이런 식으로 말이다.
마지막 프린트문은 행을 나눠 모든 카드를 반환한다.
NameA(cardType='C', cardNum='2') 
NameA(cardType='C', cardNum='3') 
NameA(cardType='C', cardNum='4')
...이런 식이다.

---