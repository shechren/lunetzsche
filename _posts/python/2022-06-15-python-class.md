---
title: Python - 클래스(Class)
date: 2022-06-15 11:20:26 +/-09:00
category: [python/basic]
tag: [python, class]
---

객체지향에서 클래스란 가장 핵심적인 요소다. 하지만 여러 강의사이트에서 듣는다 한들 클래스란 쉽사리 이해되지 않는 존재다. 우습게도 C 계열 언어에서의 class 언어가 더 쉬워보이는 경우까지 생긴다. 실제로 본인은 C# 에서의 클래스는 쉽게 이해했지만 Python 에서의 클래스는 잘 이해하지 못했다.

클래스의 정의란 우리가 목표로 하는 과정 속에서 무엇인가가 필요할 것이다. 그 필요한 것들을 담아주는 보따리와 같은 존재다. 도라에몽 도와줘! 하면 도라에몽이 배의 주머니에서 무언가를 주섬주섬 꺼내는 것을 생각하면 이해가 빠를 것이다. 우리는 이 클래스 안에 오만가지 변수(str, int)와 데이터 형식(list, tuple, dict, set)과 함수(def) 등을 넣어놓고 원하는 때에 꺼내서 쓸 수 있다.

아래를 보자.

```python
abList = []
for i in range(97, 123):
  abList.append(chr(i))
print(a.abList)
```
코드가 조금 어수선하지만 신경쓰지 말자. 어차피 조금 이따가 지능형 리스트로 지울 것이다.
이 코드는 함수도 사용하지 않고 클래스도 사용하지 않은 일반적인 코드다. chr를 본다면 이해가 쉬울 테지만, 이 코드는 a부터 z까지 순서대로 나열해주는 코드다. 즉, ASCII 코드의 97에서 122까지를 불러와주는 것이다.
이것을 클래스에 넣는다면 어떻게 될까? 이왕 넣는 김에 지능형 리스트를 이용하여 저 코드의 길이도 줄여보자.
```python
class Alphabet:
  abList = [chr(i) for i in range(97, 123)]
a = Alphabet()
print(a.abList)
```

우리는 a를 Alphabet이란 클래스에 연동시켜두었다. 그럼 이제 a는 Alphabet에 들은 것들을 내가 원하는 때에 마음대로 꺼내서 쓸 수 있는 것이다. 이 클래스는 이제 제 겁...
그래서 우리는 Alphabet 안에 들은 abList를 꺼내 프린트를 했다.

![python-class-1.png](/assets/postingImage/python-class-1.png)

이것을 그림으로 표현하면 위와 같다는 말이 되시겠다.

클래스 안에 들은 변수 또는 데이터형식 등은 Class Attribute라고 한다. 그럼 클래스 내의 함수 내에 들은 건 뭐라고 할까?

---

이제 클래스 내에 함수를 추가할 것이다. 함수는 다들 알다시피 def로 추가한다.

```python
def opq(self):
```

놀랍게도, 괄호를 여는 순간 자동으로 self가 추가된다. 클래스 내에 들은 함수는 전부 self를 상속받는다.
우리는 여기서 멈추지 않고 song이라는 매개변수(parameter)를 하나 더 받아올 것이다. 물론 self가 추가된 이상 그냥 쓸 수는 없다.

```python
def opq(self, song):
  self.opq = song
```
여기서 추가된 song, 그러니까 함수 내에 들은 형식은 바로 Instance Attribute라고 한다.

이제 우리는 parameter를 받아왔다. 인풋이 있으니 아웃풋도 있어야겠지? argument도 만들어야 한다.
그 전에, song을 실행시키면 무엇이 실행될 것인지 또한 중요하다.

```python
if song == ord("o"):
  print("opqrstu~")
else:
  pass
```
song이 o에 해당하는 ord일 경우 출력하고 아닐 경우 패스한다.
o에 해당하는 ord는 111이다. 우리는 111을 입력하면 출력으로 노래를 이어 할 것이다.

```python
a = Alphabet()
a.opq(int(input("111을 입력해주세요.")))
```

**자. a.opq()부분을 주목해야 한다. 저기서 저 괄호 안이 song이 들어갈 자리. 다시 말해 argument다.**
a에 알파벳 클래스를 대응시켜놓았으니 아웃풋만 만들면 된다. 우리는 아웃풋 a.opq(song)을 만들었고,
song에는 우리가 입력시켜두는 인풋을 대응시켜놓은 것이다.

```python
class Alphabet:
  #abList = [chr(i) for i in range(97, 123)]
  def opq(self, song):
    self.song = song
      if song == ord("o"):
        print("opqrstu~")
      else:
        pass
a = Alphabet()
a.opq(int(input("111을 입력해주세요.")))
```

이제 나머지는 실행만 시켜보면 된다.
우리는 클래스(1행)을 건너뛰어 11행과 12행으로 넘어온 것을 알 수 있다.
중간에 인풋을 넣으라고 한다. 111을 넣어보자.

![python-class-2.png](/assets/postingImage/python-class-2.png)

우리가 아웃풋에서 넘긴 song, 그러니까 111이 4열의 의 함수 opq의 song 자리에 인풋되었다.

![python-class-3.png](/assets/postingImage/python-class-3.png)

그리고 제대로 출력되는 것을 확인할 수 있다.

---