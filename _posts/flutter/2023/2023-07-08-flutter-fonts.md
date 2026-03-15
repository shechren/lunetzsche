---
title: Flutter - Fonts
date: 2023-07-08 13:47:57 +/-09:00
category: [flutter/basic]
tag: [flutter, fonts]
---

여기서는 나눔손글씨 폰트를 사용할 것이다.

내장 폰트는 다음과 같이 쓸 수 있다.

1\. 폰트를 에셋 폴더 내에 추가한다.

2\. pubspec.yaml 파일의 경로를 추가한다.
```yaml
flutter:
  uses-material-design: true
  assets:
    - Fonts/NanumPen.ttf
  fonts:
    - family: NanumPen
      fonts:
        - asset: Fonts/NanumPen.ttf
```
3\. 텍스트에 폰트를 넣는다.
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar: AppBar(
            title: Text("images Loading"),
          ),
          body: Text("폰트를 바꿨어요!",
          style: TextStyle(fontSize: 50,fontFamily: "NanumPen"))),
    );
  }
}
```

![flutter-fonts-1.png](/assets/postingImage/flutter-fonts-1.png)

---