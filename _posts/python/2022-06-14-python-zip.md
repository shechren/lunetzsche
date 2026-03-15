---
title: Python - Zip
date: 2022-06-14 11:16:23 +/-09:00
category: [python/basic]
tag: [python, zip]
---

여기서는 제네레이터 함수 zip을 설명한다.

zip 메소드는 같은 인덱스를 가진 두 개의 iter(반복 가능한 객체)를 하나로 합쳐준다.

보면 쉽다. 설명이 필요없다. 백문이 불여일견. 일단 보자.
```python
myList1 = ["Qiqi", "Mimi", "Nana"]
myList2 = ["Yorkshire Terrier", "Spitz", "Mix"]
mergeList = list(zip(myList1, myList2))
print(mergeList)
[('Qiqi', 'Yorkshire Terrier'), ('Mimi', 'Spitz'), ('Nana', 'Mix')]
```

너무 깔끔하게 묶어져 나왔다.

---

이제 이 zip을 응용해보자.

일단 이것을 새로운 리스트로 그대로 복사한다.

```python
newList = [('Qiqi', 'Yorkshire Terrier'), ('Mimi', 'Spitz'), ('Nana', 'Mix')]
```

그리고 빈 딕셔너리 하나를 만든다.

```python
newDict = {}
```

List to Dict, 그러니까 이 리스트를 딕셔너리로 형식변환할 수 있는 방법은 없을까? 있다. for문을 사용하면 된다.

```python
for i in newList:
  print(i)
```

먼저 이렇게 하면 결과값이 다음과 같이 나올 것이다.

```text
('Qiqi', 'Yorkshire Terrier') 
('Mimi', 'Spitz') 
('Nana', 'Mix')
```

그럼 여기서 인덱스를 분류해서 따로 저장한다.

```python
a = i[0]
b = i[1]
```

이런 식으로 말이다. a는 키, b는 밸류가 될 것이다.

다시 처음으로 돌아가서, 이걸 정리해보자.

```python
myKeys = []
myValues = []
for i in newList:
  a = i[0]
  b = i[1]
  myKeys.append(a), myValues.append(b)
print(myKeys, myValues)
```

```text
['Qiqi', 'Mimi', 'Nana'] ['Yorkshire Terrier', 'Spitz', 'Mix']
```

자 이제 우리의 키와 밸류가 별도로 저장되어 빠져나온 것을 알 수 있다.

이것을 dict으로 만드려면...
```python
for j in range(len(myKeys)):
  newDict[myKeys[j]] = myValues[j]
print(newDict)
```
myKeys의 길이의 범위는 3이다.

**newDict에는 myKeys[j]가 있다. 거기에  myValues[j]를 대입한다.**

뭔 말인지 모르겠다고? 그럼 정상이다.


아래는 완성된 코드다. 이 코드를 아래 사이트에 가져가보자. 저 사이트에서 그래픽을 보면 한결 이해가 쉬워진다.

```python
newList = [('Qiqi', 'Yorkshire Terrier'), ('Mimi', 'Spitz'), ('Nana', 'Mix')]
newDict = {}
myKeys = []
myValues = []

for i in newList:
  a = i[0]
  b = i[1]
  myKeys.append(a), myValues.append(b)

for j in range(len(myKeys)):
  newDict[myKeys[j]] = myValues[j]
print(newDict)
```

![python-zip.png](/assets/postingImage/python-zip.png) 

하여튼 대강 뭐가 뭔지는 알았다. 근데 문제는 무진장 복잡하다. 무진장 복잡하다...

---

솔직히 이게 말이 되는 코드냐고? 그럴리가 없다. 그렇다면 우린 어떻게 해야 할까?

zip을 이용하면 된다. key들을 i, value들을 j에 인계해서 그대로 언패킹하고, myDict은 같은 방식으로 진행한다.

```python
myList1 = ["Qiqi", "Mimi", "Nana"]
myList2 = ["Yorkshire Terrier", "Spitz", "Mix"]
myDict = {}
for i, j in zip(myList1, myList2):
  myDict[i] = j
print(myDict)
```

결과를 출력해보자.

```text
{'Qiqi': 'Yorkshire Terrier', 'Mimi': 'Spitz', 'Nana': 'Mix'}
```

놀랍게도 하나의 Dict이 이렇게 완성된다. 이쯤 되면 거의 지능형 딕셔너리 수준이다.

이 코드도 가능하면 위 사이트에 가져가서 그래픽으로 본다면 이해하기 쉬울 것이다.

다시 말해 for문을 돌려서 할 수도 있지만, zip을 사용하면 보다 파이썬스러운(Pythonic) 과정과 결과를 얻을 수 있다.

---
