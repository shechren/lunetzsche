---
title: Python - Numpy (1)
date: 2023-11-04 20:01:52 +/-09:00
category: [python/NumPy]
tag: [python, opencv, numpy]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

파이썬은 행렬(Matrix)을 표현하는 기본 자료형이 없다. 그래서 Numpy 라이브러리를 이용하게 된다.
파이썬 OpenCV 배열은 Numpy로 변환된다. 이밖에도 SciPy나 Matplotlib도 OpenCV에서 사용할 수 있긴 하다.
우리가 만날 구조는 ndarray인데, ndim, shape, dtype이 주요 요소다.
* ndim은 차원의 수를 나타낸다. 몇차원 배열인지 알 수 있다.
* shape는 차원의 크기다. 이미지의 너비, 높이, 채널을 의미한다.
* dtype는 데이터 형식이다. 정밀도를 설정하는 역할을 한다.

이미지는 흔히들 2차원이라고 부르지만 파이썬에서는 3차원이다. 채널값이 있기 때문에 이미지는 너비와 높이, 채널을 가진다.

```python
import numpy as np

my_new_array = np.array([[1, 2, 3],
                        [0, 1, 2],
                        [0, 0, 1]])

print(my_new_array.ndim)
print(my_new_array.shape)
print(my_new_array.dtype)
```

이 행렬은 2차원 행렬이며, 크기는 3 * 3이고, 데이터 형식은 int32다.

---

```python
my_new_array = np.array(object, dtype=True, copy=True, order='K', subok=False, ndmin=0)
```
* dtype은 배열의 자료형인데, 1과 0으로 구성된 배열이라면 int32형태로 생성한다. 하지만 bool로 지정할 경우 True 혹은 False의 값을 가진 Numpy 배열을 생성한다.
* copy는 객체를 복사해서 생성할지 선택할 수 있다. False일 경우 자료형이나 순서를 고려해서 객체를 복사할지 선택한다. 자료형이나 형태가 다르면 False여도 객체를 복사해서 생성한다.
* order는 메모리 레이아웃을 설정한다. C스타일과 Fortran 스타일로 나뉜다.
C에서 \[i\]\[j\]\[k\]순서로 색인이 있다며, k부터 순차적으로 증가하고 그다음은 앞의 값이 증가한다.
반대로 Fortran은 i값부터 순차적으로 증가한다.
copy=False를 기준으로, 'K' 는 레이아웃에 최대한 일치, 'A'는 포트란에 근접한 경우 F order, 아닐 경우 C order다. C는 C order, F는 Fortran order다.

아래는 ChatGPT가 대답한 C와 Fortran의 메모리 레이아웃 스타일이다.
> ### C:
> - **포인터 기반의 언어**: C 언어는 포인터를 활발하게 사용하는 언어로, 메모리 주소를 직접 다룹니다.
> - **배열의 메모리 구조**: C에서 배열은 연속된 메모리 블록으로 저장됩니다. 다차원 배열은 1차원 배열로 메모리에 연속적으로 배치됩니다.
> - **포인터 산술(pointer arithmetic)**: C에서 포인터 산술을 통해 메모리의 이동과 접근이 가능합니다. 이로 인해 메모리 주소를 직접 조작할 수 있습니다.
> - **기본적으로 로우 메이저 (row-major)**: C는 다차원 배열을 기본적으로 로우 메이저로 저장합니다. 이는 행을 먼저 저장하고, 그 다음에 열을 저장합니다.
> 
> ### Fortran:
> - **배열 기반의 언어**: 포트란은 배열 중심의 언어로, 배열을 다루는 데 매우 효과적입니다.
> - **기본적으로 컬럼 메이저 (column-major)**: 포트란은 다차원 배열을 컬럼 메이저로 저장합니다. 이는 열을 먼저 저장하고, 그 다음에 행을 저장합니다.
> - **배열의 메모리 구조**: 다차원 배열의 저장 순서가 C와 반대로 되어 있어, 포트란에서는 주로 열 단위로 데이터에 접근합니다.
> - **포인터 사용의 제한**: 포트란은 C 언어보다 포인터 사용에 제약을 두는 경향이 있습니다.
> 
> 이러한 메모리 레이아웃의 차이는 주로 다차원 배열의 요소에 접근하는 방식이나 데이터 저장 방식에 영향을 미칩니다. 따라서 C와 포트란 간의 데이터 교환 시, 배열의 요소를 정확하게 해석해야 하며, 저장 순서에 주의해야 합니다.

* subok는 하위 클래스에서 배열을 생성할지에 대한 여부다. True면 하위 클래스에 전달되고, False라면 반환된 배열이 ndarray 클래스가 된다.
* ndmin은 반환된 배열의 최소 차원 수를 나타낸다. 입력을 1차원이나 2차원으로 해도 ndmin이 3이면 3차원 배열이 되고, 2로 하면 최소 2차원 배열로 생성된다.

---

다음은 ndarray 클래스의 배열 생성 함수다. 다 적지는 않고 중요한 부분만 적을 것이다. 없는 부분은 공식 레퍼런스나 서적을 참조하자.

```python
np.eye(n, m, k=0, dtype=None)
```
n * m 크기의 k만큼 오프셋된 단위 행렬 생성
```python
np.identity(n, dtype=None)
```
n * n 크기의 단위 행렬 생성
```python
np.ones([n, m, ...], dtype=None)
1로 채워진 배열 생성
```python
np.ones_like(object, dtype=None)
```
해당 배열 크기와 동일한 크기의 배열인데 전부 1로 채워 생성
```python
np.zeros([n, m, ...], dtype=None)
0으로 채워진 배열 생성
```python
np.zeros_like(object, dtype=None)
```
해당 배열 크기와 동일한 크기의 배열인데 전부 0으로 채워 생성
```python
np.full([n, m, ...], fill_value, dtype=None)
```
fill_value로 채워진 배열 생성
```python
np.full_like(object, fill_value, dtype=None)
```
해당 배열 크기와 동일한 크기의 배열인데 전부 fill_value로 채워 생성
```python
np.diag(v, k=0)
```
2차원 배열 이하 v 배열을 k만큼 오프셋된 대각 행렬을 생성

---