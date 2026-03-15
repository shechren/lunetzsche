---
title: Python - 자료 구조형들의 언패킹(unpacking)
date: 2022-06-14 10:16:39 +/-09:00
category: [python/basic]
tag: [python, unpacking]
---

- [**1. 기본 언패킹**](#기본-언패킹)
- [**2. 딕셔너리(Dictionary)의 언패킹**](#딕셔너리dictionary의-언패킹)

# **기본 언패킹**
```python
tup = ("a", "b")
a, b = tup
print(a)
print(b)
```

간단하다. 설명은 생략한다.

---

## **딕셔너리(Dictionary)의 언패킹**
1\. 먼저 Dict을 준비한다.
```python
peopleDict = {
  "People1": {"Height": 175, "Weight": 65},
  "People2": {"Height": 160, "Weight": 50},
  "People3": {"Height": 180, "Weight": 75}
}
```
이 구조는 Dict 안에 또다른 Dict이 들은 것이니 문제 없다.

2\. 언패킹한다. 주의할 점은 dict은 set과 마찬가지로 순서가 없으니 **인덱스로 뽑아올 수 없다.**

```python
a = peopleDict.keys()
b = peopleDict.values()
c = peopleDict.items()
for i in a, b, c:
  print(i)
```
a에는 키, b에는 밸류, c는 둘 모두를 뽑아온다.

출력 결과.

**dict\_keys(['People1', 'People2', 'People3'])**
**dict\_values([{'Height': 175, 'Weight': 65}, {'Height': 160, 'Weight': 50}, {'Height': 180, 'Weight': 75}])**
**dict\_items([('People1', {'Height': 175, 'Weight': 65}), ('People2', {'Height': 160, 'Weight': 50}), ('People3',**
**{'Height': 180, 'Weight': 75})])**



또다른 방법

for문 자체에서 언패킹을 한다.
```python
for keys, values in peopleDict.items():
  print(keys)
```
이런 식으로.

필요없는 요소들에 대해 제외하는 법은 에스터리스크를 이용한다.

---