---
title: Mathematics - 이항 정리(Binomial Theorem)
date: 2023-11-28 13:54:41 +/-09:00
category: [mathematics]
tag: [mathematics, probability, statistics]
---

- [**이항 정리**](#이항-정리)
- [**파스칼의 삼각형**](#파스칼의-삼각형)
- [**표본 공간**](#표본-공간)

---

# **이항 정리**

이항 정리는 n개의 아이템 중에서 k개의 아이템을 선택하는 경우의 수다.

$$ (a + b)^n = \sum_{k=0}^n \, (_k^n)a^{n-k}b^k $$

여기서, 이항 계수는

$$ (_k^n) = \frac{n!}{k!(n-k)!} $$

이항 정리를 사용하여 전개해보자.

$$ (x + y)^3 = (_0^3)x^3y^0 + (_1^3)x^2y^1 + (_2^3)x^1y^2 + (_3^3)x^0y^3 $$

이를 계산하면,

$$ x^3 + 3x^2y + 3xy^2 + y^3 $$

---

# **파스칼의 삼각형**

이항 계수를 나타내는 삼각형 형태의 배열을 바로 파스칼의 삼각형이라고 부른다.
해당하는 수는 자기 머리 위의 수 2개를 더해서 나온 결괏값이다.

```
      1
     1 1
    1 2 1
   1 3 3 1
  1 4 6 4 1
```

규칙은 다음과 같다.
1. 맨 위와 양쪽 끝의 수는 항상 1이다.(꼭짓점은 항상 1)
2. 각 내부의 숫자는 위의 두 숫자를 더한 값이다.

n개의 아이템 중 k개의 아이템을 선택할 때,

$$ (\frac{n}{k}) $$
라고 적으며, n번째 행의 k번째 숫자라고 적는다.


예: 6개의 아이템 중 3개의 아이템을 선택한다.

$$ (\frac{6}{3}) $$

```
      1
     1 1
    1 2 1
   1 3 3 1
  1 4 6 4 1
 1 5 10 10 5 1
1 6 15 20 15 6 1
```

10 + 10 = 20이다. 꼭대기 꼭짓점을 포함해서, 모서리들의 1은 행과 열에서 제외한다.

---

# **표본 공간**

* 어떤 확률적인 실험에서 발생할 수 있는 모든 가능한 결과의 집합

표본공간 S의 정의:

$$ S = {s_1, s_2, _{\ldots}, s_n} $$

표본공간의 크기:

$$ ∣S∣ = n $$

예컨대 주사위를 던질 경우, 표본공간의 크기는 6이다.

* 사건: 표본공간의 부분집합이다. 어떤 특정한 결과의 집합을 나타낸다.

예컨대 주사위를 던질 경우, 홀수가 나오는 사건은 {1, 3, 5}다.

따라서 각 사건이 일어날 확률은 1/2이다.

---