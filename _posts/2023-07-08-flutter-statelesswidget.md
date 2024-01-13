---
title: Flutter - StatelessWidget
date: 2023-07-08 10:04:29 +/-09:00
category: [flutter/basic]
tag: [flutter, stateless]
---

플루터의 3가지 클래스 중 하나인 StatelessWidget에 대해 설명한다.
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp();
    }
}
```
StatelessWidget은 상태를 관리하지 않는 위젯이다. 말 그대로 정적 위젯이다.

변수나 함수 등을 변경하는 건 자유로우나 변경한다 한들 화면에 적용되지 않는다.

---