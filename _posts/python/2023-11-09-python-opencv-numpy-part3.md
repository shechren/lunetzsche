---
title: Python - Numpy (3)
date: 2023-11-09 17:50:06 +/-09:00
category: [python/NumPy]
tag: [python, opencv, numpy]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

NumPy에서는 브로드캐스팅(broadcasting)과 형식 캐스팅(type casting)이라는 개념을 지원하는데, 서로 차원이 다른 배열에서 연산하는 것을 말한다. 형식 캐스팅은 두 배열의 자료형(dtype)을 비교해 표현 범위가 더 넓은 자료형을 선택한다.

브로드캐스팅에는 규칙이 있다.
> 1. 두 배열의 차원(ndim)이 다르면 차원이 낮은 배열이 차원이 높은 배열과 같은 차원의 배열로 간주된다.
> 2. 반환 배열은 차원의 수가 가장 큰 배열이다.
> 3. 연산 배열과 반환 배열의 차원 크기(shape)가 같거나 혹은 1일 때 브로드캐스팅이 가능하다.
> 4. 브로드캐스팅 배열의 차원 크기는 연산 배열들의 차원 크기에 대해 최소 공배수 값으로 사용한다.

그 외의 상세 내용은 상술한 책을 참조하길 바란다.

---

이외에도 배열의 함수들 중 일부를 서술한다.

```python
np.add(array1, array2)
np.subtract(array1, array2)
np.multiply(array1, array2)
np.divide(array1, array2)
```
사칙연산이다. 각각 덧셈, 뺄셈, 곱셈, 나눗셈이다.

```python
np.power(array1, array2)
```
제곱이다.

```python
np.mod(array1, array2)
```
나눗셈의 나머지다.

```python
np.floor_divide(array1, array2)
```
나눗셈을 내림처리한다.

```python
np.logaddexp(array1, array2)
```
지수의 합을 로그(log) 처리한다.

```python
np.logaddexp2(array1, array2)
2의 제곱의 합을 밑이 2인 로그로 처리한다.
```

```python
np.positive(array)
```
양수를 곱한다.

```python
np.negative(array)
```
음수를 곱한다.

```python
np.abs(array)
```
절댓값

```python
np.round(array)
```
반올림

```python
np.ceil(array)
```
올림

```python
np.floor
```
내림

```python
np.maximum(array1, array2)
```
최댓값

```python
np.minimum(array1, array2)
```
최솟값

```python
np.sqrt(array)
```
제곱근

```python
np.exp(array)
```
지수

```python
np.log(array)
```
밑이 e인 로그

---

삼각 함수도 일부 적는다.

```python
np.sin(array)
np.arcsin(array)
```
사인과 아크사인

```python
np.cos(array)
np.arccos(array)
```
코사인과 아크코사인

```python
np.tan(array)
np.arctan(array)
```
탄젠트와 아크탄젠트

```python
np.arctan2(array1, array2)
```
아크 탄젠트(array1 / array2)

```python
np.deg2rad(array)
```
각도에서 라디안 변환

```python
np.rad2deg(array)
```
라디안에서 각도 변환

```python
np.hypot(array1, array2)
```
빗변 계산

---