---
title: Python - Numpy (2)
date: 2023-11-09 17:29:24 +/-09:00
category: [python/NumPy]
tag: [python, opencv, numpy]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

자동으로 배열을 생성해보자. arange 메소드의 도움을 받을 수 있다.

```python
import numpy as np

array = np.arange(20)

print(array)
```
결과:
<b>
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19] 가 출력된다.
</b>
중요한 것은 이 배열을 어떤 형태로 가져갈 것인가에 대해서다.
reshape 메소드는 배열의 차원을 재정의할 수 있다.

```python
array2 = np.reshape(array, (5, 4))
print(array2)
```

결과:
<b>
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]
 [12 13 14 15]
 [16 17 18 19]]
 </b>

 2차원 배열로, 5개의 행과 4개의 열이 생성되었다.

```python
array3 = np.reshape(array2, (5, 2, 2))
print(array3)
```

결과:
<b>
[[[ 0  1]
  [ 2  3]]

 [[ 4  5]
  [ 6  7]]

 [[ 8  9]
  [10 11]]

 [[12 13]
  [14 15]]

 [[16 17]
  [18 19]]]
  </b>

2차원 배열을 다시 손질하여, 3차원 배열로 만들었다.

```python
array4 = np.reshape(array2, (4, -1))
print(array4)
```

여기서 -1은 원래 배열의 크기를 알아내면 그 다음 값을 자동으로 추론한다.
array2는 5개의 행과 4개의 열을 가지고 있다. 즉, 5×4의 크기다.
20에서 먼저 들어온 4를 나누면 5가 남는다. 따라서 array4는 4개의 행과 5개의 열을 가지게 된다.


---

new_axis를 통해 배열의 차원만 증가시키는 것이 가능하다.

```python
new_axis = array[np.newaxis]
print(new_axis)
```
결과:
<b>
[[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]]
</b>

flatten을 통해 배열의 차원을 축소시키는 것도 가능하다.

```python
flatten = new_axis.flatten()
print(flatten)
```

결과:
<b>
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
</b>


배열 두 개를 병합시키는 것이 가능하다. 동형 행렬이 필요하다. 동형 행렬은 페이지, 행, 열이 같은 행렬을 의미한다.
즉, 2×2 행렬 두 개는 서로 동형 행렬이다.

```python
array = np.array([[0, 1], [2, 3]])
array2 = np.array([[4, 5], [6, 7]])

merge1 = np.stack([array, array2], axis=0)
print(merge1)

merge2 = np.stack([array, array2], axis=1)
print(merge2)
```

stack 메소드를 이용하면 동형 행렬을 서로 병합해서 새로운 배열을 반환한다.
중요한 것은 axis인데, 행렬의 방향을 결정한다. axis0은 수직 방향으로 행을 추가한다. 반대로 axis1은 수평 방향으로 열을 추가한다.

결과:
axis0 ->
[[[0 1]
[2 3]]

[[4 5]
[6 7]]]

axis1 ->
[[[0 1]
[4 5]]

[[2 3]
[6 7]]]

반대로, 나누는 것은 split 메소드를 사용한다.
```python
array = np.array([[0, 1, 2], [3, 4, 5]])
array2 = np.array([[0, 1], [2, 3], [4, 5]])

split1, split2 = np.split(array, 2, axis=0)
split3, split4, split5 = np.split(array, 3, axis=1)
split6, split7 = np.split(array, 2, axis=0)

print(split1)
print(split2)

print(split3)
print(split4)
print(split5)

print(split6)
print(split7)
```

결과:
<b>
[[0 1 2]]
[[3 4 5]]

[[0]
 [3]]
[[1]
 [4]]
[[2]
 [5]]

[[0 1 2]]
[[3 4 5]]
</b>

---