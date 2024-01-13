---
title: Python - OpenCV - 도형 그리기 (1)
date: 2023-11-03 22:03:09 +/-09:00
category: [python/OpenCV]
tag: [python, opencv]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

도형은 선형 타입과 비선형 타입으로 나뉜다.
선형 타입으로 그리는 방법에는 Bresenham's algorithm, Anti-Aliasing, 내부 채우기 방식 등이 있다.

우리는 먼저 <b>직선</b>을 그려볼 것이다.

약간 수학적인 이야기를 하자면, <b>직선의 방정식</b>을 쓰면 우리는 두 점 사이의 모든 점에 대해 좌표를 알 수 있다. 이게 왜 중요하냐면 점과 점이 연결된 것이 선이고, 선은 무수한 점들의 연속으로 이루어져 있기 때문이다.
직선의 방정식은 y = mx + b이다. m은 기울기를 나타낸다. b는 y축 절편을 의미한다.
그런데 문제는 점의 좌표는 모두 정수인데, 직선의 방정식을 쓰는 순간 어떻게든 실수값이 등장한다는 것이다.
예컨대 0과 1 사이에 무수히 많은 실수가 존재함을 생각해보면 이해가 쉽다. 그래서 정수만을 이용해서 선을 그릴 수 있도록 개발된 알고리즘이 바로 브레젠험 알고리즘이다. 정수만을 이용해서 동작하기에 성능적으로 매우 우수한 반면, 직선이 아닌 곡선을 그리는데는 적합하지 않은 알고리즘이다.

---

도형을 그리기에 앞서, 마우스 콜백(Mouse Callback) 함수를 먼저 작성해두자.
이 함수에 대해서는 나중에 다른 포스팅으로 또 적을 테지만, 일단은 대략적으로 똑같이 적으면 된다.
```python
import cv2
import numpy as np


def event(event, pos_x, pos_y, flags, param):
    if event == cv2.EVENT_LBUTTONDOWN:
        my_x_pos = pos_x
        my_y_pos = pos_y
        print(my_x_pos, my_y_pos)


image = cv2.imread('1.png')
cv2.namedWindow('image', flags=cv2.WINDOW_FREERATIO)
cv2.resizeWindow('image', 500, 500)
cv2.imshow('image', image)
cv2.setMouseCallback('image', event, param=None)
cv2.waitKey(0)
```
콜백은 setMouseCallback 메소드를 이용하는데, 여기서 받는 event 함수에는 5개의 매개변수가 필요하다.
먼저 마우스 이벤트 타입을 받고, 마우스 이벤트가 발생한 좌표 x축과 y축을 받는다. flags와 param는 여기서는 사용되지 않는다.

이미지를 불러오고, 이미지의 위치를 클릭할 때마다 x좌표와 y좌표가 int 형식으로 출력된다.
이제 우리는 이것을 선을 그릴 것이다.

---

두 점 사이에 선을 긋는 메소드는 이것을 사용한다.
```python
cv2.line(target_image, (first_x_pos, first_y_pos), (second_x_pos, second_y_pos), (0, 0, 0), 3, cv2.LINE_AA)
```
이미지에 그릴 것인데, 첫 x, y축 좌표와 두번째 x, y축 좌표를 받아와 그 사이에 검은색 선을 그린다. 두께는 3이다.

```python
import cv2

first_x_pos, first_y_pos = None, None
second_x_pos, second_y_pos = None, None


def event(event, pos_x, pos_y, flags, param):
    global first_x_pos, first_y_pos, second_x_pos, second_y_pos

    if event == cv2.EVENT_LBUTTONDOWN:
        if first_x_pos is not None and second_x_pos is not None:
            first_x_pos, first_y_pos = None, None
            second_x_pos, second_y_pos = None, None

        if first_x_pos is None:
            first_x_pos = pos_x
            first_y_pos = pos_y
        else:
            second_x_pos = pos_x
            second_y_pos = pos_y

    if first_x_pos is not None and second_x_pos is not None:
        cv2.line(image, (first_x_pos, first_y_pos), (second_x_pos, second_y_pos), (0, 0, 0), 3, cv2.LINE_AA)
        cv2.imshow('image', image)
        print(f'첫번째 점: {first_x_pos, first_y_pos}, 두번째 점: {second_x_pos, second_y_pos}')
        first_x_pos, first_y_pos = None, None
        second_x_pos, second_y_pos = None, None

image = cv2.imread('1.png')
cv2.namedWindow('image', flags=cv2.WINDOW_FREERATIO)
cv2.resizeWindow('image', 500, 500)
cv2.imshow('image', image)
cv2.setMouseCallback('image', event, param=None)
cv2.waitKey(0)
```

두 점의 좌표를 기록할 변수를 만들고, 콜백 이벤트를 만든다.
첫 x좌표가 기록되면 다음에는 두번째 x좌표가 기록되게 한다. y좌표도 마찬가지이지만 검사는 x만 해도 충분하다. y는 동시에 기록되니까 x만 검사해도 y의 유무를 알 수 있기 때문이다.
x좌표가 둘 모두 기록되면, line 메소드를 사용한다.
image를 지정하고, 두 점을 지정하고, 색깔과 두께를 지정한다. 선의 타입도 지정해준다.
마지막으로 모든 좌표를 초기화해주면 된다.

![python-opencv-1.jpg](/assets/postingImage/python-opencv-1.jpg)

---