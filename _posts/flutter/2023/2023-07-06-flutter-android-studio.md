---
title: Flutter - 안드로이드 스튜디오(Android Studio)
date: 2023-07-06 11:27:50 +/-09:00
category: [flutter/basic]
tag: [flutter, android studio]
---

![flutter-android-studio-1.png](/assets/postingImage/flutter-android-studio-1.png)

Flutter 설치는 ChatGPT 및 Wrtn과 같은 인공지능에 물어보면 상세히 잘 가르쳐주니 별도로 여기에 적지는 않을 것이다.

안드로이드 스튜디오를 열면 다음과 같은 창이 나온다. 전체적인 인터페이스는 JetBrain 사가 만들었기에 같은 회사의 IntelliJ 및 Pycharm과 매우 흡사하다.

lib 폴더의 main.dart 파일이 우리의 메인 파일이라고 생각하면 될 것이다.

이제 코드를 아래와 같이 바꿔보자.
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("this is my first Application"),
        ),
        body: Center(child: GestureDetector(child: Text("Hello!"))),
      ),
    );
  }
}
```

그리고 디바이스 매니저에서 디바이스를 설정한다.

![flutter-android-studio-2.png](/assets/postingImage/flutter-android-studio-2.png)

실행하면 다음과 같이 뜬다. 초기 설정 때문에 실행이 정말 오래 걸린다. 인내심을 갖고 기다리자.

![flutter-android-studio-3.png](/assets/postingImage/flutter-android-studio-3.png)

탭하면 첫 애플리케이션이라는 문장이 등장한다.

---