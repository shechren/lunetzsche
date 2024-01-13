---
title: Python - 모듈(Module)
date: 2022-06-15 11:46:14 +/-09:00
category: [python/basic]
tag: [python, module, library]
---

파이썬 내장 모듈에는 여러가지가 있다. datetime이라든지 random이라든지...

우리는 일단 테스트용으로 datetime을 써보자.

```python
import datetime
```

여기서 우리가 datetime. 을 쓰고 콤마 옆에 커서를 대보면 datetime이 제공하는 정말 많은 메소드(Method)가 표시된다.



그런데 만약 나는 datetime의 date 메소드만 쓰고 싶어. 하지만 다른 건 다 필요하지 않아! 하는 경우도 종종 생긴다. 그럴 경우 이런 방식으로 임포트한다.
```python
from datetime import date
```

이렇게 하면 date 메소드만 골라서 사용할 수 있게 된다.

```python
from datetime import \*
```
이런 메소드도 있다. 이 경우 파이썬이 기본적으로 제공하는 메소드 위에 모든 것을 오버라이딩한다. 다시 말해 기본 메소드 위에 datetime 전체를 덮어씌우는 것이다. 그리 권장되지는 않는 방법이다.

아무튼 datetime을 임포트했는데 그때마다 datetime을 치기가 불편하고 귀찮다. 그럴 때도 방법이 있다.

```python
import datetime as dt
```
이렇게 하고 dt. 를 치고 콤마 옆에 커서를 올려보면 상술한 datetime과 같은 메소드들이 나타난다.

---
