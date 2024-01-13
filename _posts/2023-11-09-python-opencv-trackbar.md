---
title: Python - OpenCV - 트랙바
date: 2023-11-09 20:52:14 +/-09:00
category: [python/OpenCV]
tag: [python, opencv]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

TrackBar는 우리가 이미지를 쉽게 편집하기 위한 GUI를 제공한다.

```python
import numpy as np
import cv2

def create_image(b, g, r):
    return np.full((500, 500, 3), (b, g, r), dtype=np.uint8)

def on_change_blue(pos):
    global b
    b = pos
    b = cv2.getTrackbarPos('Blue', 'new_image')
    cv2.imshow('new_image', create_image(b, g, r))

def on_change_green(pos):
    global g
    g = pos
    g = cv2.getTrackbarPos('Green', 'new_image')
    cv2.imshow('new_image', create_image(b, g, r))

def on_change_red(pos):
    global r
    r = pos
    r = cv2.getTrackbarPos('Red', 'new_image')
    cv2.imshow('new_image', create_image(b, g, r))

b, g, r = 0, 0, 0
cv2.namedWindow('new_image')
cv2.createTrackbar('Blue', 'new_image', 0, 255, on_change_blue)
cv2.createTrackbar('Green', 'new_image', 0, 255, on_change_green)
cv2.createTrackbar('Red', 'new_image', 0, 255, on_change_red)

cv2.imshow('new_image', create_image(b, g, r))
cv2.waitKey(0)
cv2.destroyAllWindows()
```
코드를 하나씩 분석해보자.

* b, g, r은 말 그대로 Blue, Green, Red의 값이다. 초기값은 0이다.
* create_image: np.full()을 사용한다. full은 n, m, fill_value, type을 인자로 받는다.
 n은 x축, m은 y축, fill_value는 b, g, r, type은 부동소수점으로 받는다.
* getTrackBarPos는 트랙바의 움직임을 얻는다. 우리는 new_image의 트랙바 움직임을 얻어서 on_change의 b, g, r 값에 넣을 것이다.
* createTrackBar는 트랙바를 만든다. name, namedWindow, initial_value, maximum_value, 마지막으로 함수를 지정한다.

---