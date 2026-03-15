---
title: Flutter - Do it Myself
date: 2023-07-09 13:49:31 +/-09:00
category: [flutter/basic]
tag: [flutter]
---

개떡같은 JS는 때려치우고 Flutter와 행복 코딩을 즐길 시간이다.

![flutter-designed-myself.png](/assets/postingImage/flutter-designed-myself.png)

위와 같은 화면을 만드는 게 목표다. 어떻게 해야 할까?

[https://github.com/shechren/DIM/tree/master/Do1](https://github.com/shechren/DIM/tree/master/Do1)

전체 코드는 여기에 있다.

일단 머티리얼 앱을 만든다. 

```dart 
    debugShowCheckedModeBanner: false,
          home: Scaffold(
              appBar: AppBar(
                title: const Text("고립은둔청년네트워크"),
              ),
```
debugShowCheckedModeBanner: 디버그 배너를 없앤다.

앱바에서 앱 이름을 적는다.
```dart
    body:
      Container(
        decoration: const BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
            colors: [Colors.black12, Colors.black])
        ),
        width: 1000,
        height: 1000,
```
바디에는 일단 다음과 같이 적는다.

사각형 컨테이너가 필요하다. 데코레이션을 위와 같이 넣고, 길이와 너비를 1000씩 설정한다.
```dart
    child: const Column(
      children: [
        SizedBox(height: 20),
      Text(
        "고립은둔청년네트워크 샘플 페이지입니다.",
        softWrap: false,
        textDirection: TextDirection.ltr,
        style: TextStyle(
          color: Colors.white,
          fontSize: 20,)
      ),
        SizedBox(height: 15),
      Row (
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon (
            Icons.accessibility_rounded,
            color: Colors.green, size: 200,
          ),
          Icon (
            Icons.accessibility_rounded,
            color: Colors.greenAccent, size: 200,
          ),
          Icon (
            Icons.accessibility_rounded,
            color: Colors.lightGreen, size: 200,
          ),
        ],
      ),
      SizedBox(height: 10),
```
Column을 추가하고, 제일 먼저 여백을 추가한다. 타이틀을 적은 뒤에 다시 여백을 추가하고,

Row로 3개의 아이콘을 추가할 것이니까 children을 만든다. 사람 아이콘 3개를 추가하고 색깔을 각자 다르게 한다.

마지막으로 여백을 또 추가한다.
```dart
     Text(
      "고립은둔청년네트워크는 전국의 고립청년 및 은둔청년을 돕기 위해 활동합니다.\n"
      "현재 아직 결성된 것은 아니며 본 이름 또한 가칭을 사용합니다.\n"
      "앞으로 어떠한 일을 진행할지도 인원 구성이 어떻게 될지도 정해지지 않았지만\n"
      "우리는 앞으로 소외된 청년들이 없도록 노력하여 사회에 기여하고 싶습니다.\n"
      ,
      softWrap: false,
      textDirection: TextDirection.ltr,
          style: TextStyle(
            color: Colors.white,
            fontSize: 16
          ),)],
        ),
      ),
    ),
```
마지막으로 텍스트 관련 설정을 하면 끝이 난다.

---