---
title: Python - List의 replace
date: 2022-05-04 14:08:53 +/-09:00
category: [python/basic]
tag: [python, list, replace]
---

- [**1. Index 찾기**](#1-index-찾기)
- [**2. for문 루프**](#2-for문-루프)
- [**3. 새로운 List 생성**](#3-새로운-list-생성)
- [**4. map(lambda())**](#4-maplambda)

# **1. Index 찾기**

```python
a = ["a", "b", "c", "d", "e"]

a[0] = "z"
```

결과

```text
['z', 'b', 'c', 'd', 'e']
```

```python
a = ["a", "b", "c", "d", "e"]
a[-1] = "a"
```

결과

```text
['a', 'b', 'c', 'd', 'a']
```


# **2. for문 루프**

```python
a = ["안녕하세", "반갑습니다", "고마워요"]
for i in range(len(a)):
  if a[i] == "안녕하세":
    a[i] = "안녕하세요"
print(a[0])
```

결과

```text
안녕하세요
```


# **3. 새로운 List 생성**

```python
a = ["1", "100", "2", "3"]
b = []
for i in a:
  temp = i.replace("100", "10000")
  b.append(temp)
```

결과

```text
['1', '10000', '2', '3']
```

# **4. map(lambda())**


```python
a = ["11", "12", "13"]
a = list(map(lambda z: (z + "안녕"), a))
```
a 자체에 람다식을 넣어 매개변수 z에 "안녕"을 a에 추가한다.

결과

```text
['11안녕', '12안녕', '13안녕']
```

---