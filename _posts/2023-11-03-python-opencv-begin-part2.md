---
title: Python - OpenCV - begin (2)
date: 2023-11-03 19:45:32 +/-09:00
category: [python/OpenCV]
tag: [python, opencv]
---

본 포스팅은 이 책을 많이 참고했다. [!https://www.yes24.com/Product/Goods/99029275](https://www.yes24.com/Product/Goods/99029275)

---

윈도우 설정 방법에 대해 알아본다.
이미지 크기를 조절하는 방법부터 알아보자.

```python
import cv2

image = cv2.imread('1.png')

cv2.namedWindow('image', flags=cv2.WINDOW_FREERATIO)
cv2.resizeWindow('image', 500, 500)
cv2.imshow('image', image)
cv2.waitKey(0)
```

namedWindow로 image를 보여주는 윈도우를 만들자. 그냥 만들어버리면 resizeWindow로 이미지 크기를 아무리 조정해봤자 그대로 나와버린다.
flags에 cv2.WINDOW_FREERATIO를 지정한다.
waitKey(0)를 지정하고 실행시켜보자.
가로 500, 세로 500의 이미지가 실행된다.

```python
cv2.destroyWindow(winname)
```
cv2.destroyWindow는 윈도우를 강제로 없애준다. winname에는 string 윈도우 이름을 넣는다.

---

이미지를 수정하는 방법은 이것 뿐만이 아니다.

```python
cv2.moveWindow(winname, x, y)
```
winname의 윈도우를 x, y 위치로 이동한다. 기준축은 좌측 상단이다.(0, 0)

```python
cv2.resizeWindow(winname, width, height)
```
winname의 윈도우의 크기를 지정한다.

```python
cv2.setWindowTitle(winname, title)
```
winname 윈도우의 이름을 변경한다.

```python
cv2.setWindowProperty(winname, prop_id, prop_value)
```
winname 윈도우의 속성을 prop_id의 prop_value로 변경한다.

```python
cv2.getWindowProperty(winname, prop_id)
```

winname 윈도우의 속성 prop_id를 검색한다.

```python
cv2.startWindowThread()
```
스레드를 통해 윈도우가 자동으로 갱신된다.

```python
cv2.destroyWindow(winname)
```
winname 윈도우를 제거한다.

```python
cv2.destroyAllWindows()
```
모든 윈도우를 제거한다.

---

flags에 대해서도 알아보자.

```python
cv2.WINDOWS_NORMAL
```
윈도우 크기를 조절할 수 있다.

```python
cv2.WINDOW_AUTOSIZE
```
윈도우 크기를 조절할 수 없다. 이미지 크기와 동일해진다.

```python
cv2.WINDOW_FULLSCREEN
```
윈도우를 최대 크기까지 키운다.

```python
cv2.WINDOW_KEEPRATIO
```
이미지 비율을 (최대한) 유지

```python
cv2.WINDOW_FREERATIO
```
비율 제한이 없다면 이미지를 최대한 확장

```python
cv2.WINDOW_OPENGL
```
OPENGL 지원하는 윈도우

```python
cv2.WINDOW_GUI_NORMAL
```
이전 GUI 방식 사용

```python
cv2.WINDOW_GUI_EXPANDED
```
상태 표시줄과 도구 모음 표시

---