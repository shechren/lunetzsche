---
title: Flutter - 파이어베이스(Firebase) 2
date: 2023-08-13 15:18:00 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase]
---

flutter에서 firebase를 사용하기 위해서는 또다른 설정이 필요하다.

```terminal
flutter pub add firebase_core
flutter pub get
```

그리고 main 함수를 다음과 같이 바꿔준다.

```dart
void main() async {
  runApp(const MyApp());
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(options: DefaultFirebaseOptions.currentPlatform);
}
```
네트워크 통신이니 당연히 비동기를 사용한다.
WidgetsFlutterBinding.ensureInitialized()를 통해 초기화를 해준다.

---
