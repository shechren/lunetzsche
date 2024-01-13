---
title: Python - Numpy (4)
date: 2023-11-09 19:35:45 +/-09:00
category: [python/NumPy]
tag: [python, opencv, numpy]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

mat 메소드는 배열을 행렬(matrix) 클래스로 변환해주는 메소드다.
행렬은 배열보다 빠른 속도로 행렬 연산을 처리한다.

```python
import numpy as np


mat1 = np.mat([[2, 3], [6, 5]], int)
mat2 = np.mat([[3, 6], [4, 2]], int)

print(mat1)
print(mat2)
print('더하기:')
my_add = mat1 + mat2
print(my_add)
print('빼기:')
my_subtract = mat1 - mat2
print(my_subtract)
print('곱하기:')
my_multiply = mat1 * mat2
print(my_multiply)
print('나누기:')
my_divide = mat1 / mat2
print(my_divide)
print('제곱:')
my_power = mat1 * mat1
print(my_power)
```

결과:
<b>
[[2 3]
[6 5]]
[[3 6]
[4 2]]
더하기:
[[ 5  9]
[10  7]]
빼기:
[[-1 -3]
[ 2  3]]
곱하기:
[[18 18]
[38 46]]
나누기:
[[0.66666667 0.5       ]
[1.5        2.5       ]]
제곱:
[[22 21]
[42 43]]
</b>

---

관심 영역을 설정하는 방법에 대해서 알아보자. 의외로 간단하다.
```python
my_array = np.array([[1, 2, 3, 4, 5], [6, 7, 8, 9, 10], [11, 12, 13, 14, 15], [16, 17, 18, 19, 20], [21, 22, 23, 24, 25]])

x, y, w, h = 1, 1, 3, 3
my_new_array = my_array[x: x+w, y: y+ h]
print(my_new_array)
```
1부터 25까지 5×5의 2차원 행렬을 배열로 만들었다.
관심 영역은 슬라이싱으로 만드는데, x는 x축 좌표, y는 y축 좌표다. 둘 모두 0부터 시작한다. w는 width이며 h는 height다.
즉 여기서는 my_array 부분이 슬라이싱이 된다는 것이다.
my_array[1: 1+3, 1: 1+3]으로 해석된다. 즉, 1번째 부분 1부터 3까지(x축), 1번째 부분 1부터 3까지(y축)라는 말이 된다.
0부터 시작하니 1번째 부분은 2번째고, 3번째 부분은 4번째다. 그러니 결과는 7부터 9, 12부터 14, 17부터 19가 나오게 된다.

---