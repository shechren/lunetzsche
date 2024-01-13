---
title: Python - 이너믈레이트(enumerate)
date: 2022-06-14 10:31:27 +/-09:00
category: [python/basic]
tag: [python, enumerate]
---

읽을 때는 '이너믈레이트' 라고 읽는다. C 계열 언어에서는 enum이라는 데이터형을 많이 사용해봤을 테지만 그것과는 엄연히 다른 것이다.

enumerate는 **인덱스를 붙여주는 역할을 한다.**



예시로 모 게임의 주인공들을 일단 리스트로 열거해보자.
```python
atelierTwilight = [("Ayesha", 1), ("Escha", 2), ("Logix", 2), ("Sharlie", 3)]
```

출력해보면 저게 한 번에 모두 출력될 것이다.
```text
[('Ayesha', 1), ('Escha', 2), ('Logix', 2), ('Sharlie', 3)]
```

이제 이것을 이너믈레이트를 이용해 출력해보자.

```python
for i in enumerate(atelierTwilight):
  print(i)
```

```text
(0, ('Ayesha', 1)) 
(1, ('Escha', 2)) 
(2, ('Logix', 2)) 
(3, ('Sharlie', 3))
```

인덱스 번호가 붙어 출력되는 모습을 알 수 있다.

---