---
title: Python - OpenCV - begin (1)
date: 2023-11-03 18:34:19 +/-09:00
category: [python/OpenCV]
tag: [python, opencv]
---

openCV 설치는 다음과 같다.

```terminal
pip install opencv-python
```

이미지를 불러오는 방법은 다음과 같다.
```python
import cv2

image = cv2.imread('1.png')
```

나는 1.png 파일을 가져오기로 했다.
cv2.imread 메소드를 사용한다. 2개의 인자를 받는데, 첫째는 파일명, 둘째는 flags다.
flags에는 입력된 파일을 어떻게 해석할지에 대한 것이다. 기본값은 IMREAD_COLOR다.

flags에는 대체로 아래와 같은 것들이 있다.

```python
cv2.IMREAD_COLOR
```
다중 채널 이미지로 변환

```python
cv2.IMREAD_GRAYSCALE
```
흑백으로 반환

```python
cv2.IMREAD_ANYDEPTH
```
정밀도가 존재할 경우 16/32비트 이미지로 반환(없으면 8비트)

```python
cv2.IMREAD_ANYCOLOR
```
채널이 존재할 경우 해당 채널 수로 반환(존재하지 않을 경우 3채널)

```python
cv2.IMREAD_IGNORE_ORIENTATION
```
이미지를 회전하지 않음

---

이미지를 출력하는 방법은 다음과 같다.
```python
cv2.imshow('image', image)
cv2.waitKey(0)
```

cv2.imshow 메소드를 사용한다. imshow는 winname과 ndarray라는 인자를 받는다.
winname은 윈도우의 이름이다. ndarray는 이미지의 행렬이다.

하지만 이미지는 곧장 종료되는데, 이를 방지하기 위해 waitKey(0)을 넣어준다. 지정된 밀리세컨드를 대기한다. 0이나 음수를 넣으면 키가 입력될 때까지 기다리게 된다.

---