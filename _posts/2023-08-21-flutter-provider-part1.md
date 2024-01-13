---
title: Flutter - Provider (1)
date: 2023-08-21 17:47:35 +/-09:00
category: [flutter/state]
tag: [flutter, state, provider]
---

Provider는 Google이 2019년에 발표한 프레임워크다.

```dart
class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Provider<int>.value(
          value: 10,
          child: const MyWidget(),
        )
      )
    );
  }
}

class MyWidget extends HookWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final myInt = Provider.of<int>(context);
    return Center(child: Text(myInt.toString()));
  }
}
```
기본적으로 위와 같은 패턴으로 사용한다.
Provider\<T\>.value는 value에 값을 저장하고 자식 위젯에 값을 넘길 수 있다.
그러면 자식 위젯에서는 해당 제네릭을 context로 받아오면 된다.
Provider는 언제나 빌드 함수 내에서 실행된다.

대표적인 Provider로 ChangeNotifierProvider가 있다.

**ChangeNotifierProvider**
```dart

class MyString with ChangeNotifier {
  String _myString = "";

  String get myString => _myString;
  
  changeMyString (value) {
    _myString = value;
    notifyListeners();
  }
}

class MyWidget extends HookWidget {
  MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final data = Provider.of<MyString>(context);
    final myController = useTextEditingController();

    return Column(
      children: [
        TextField(
          controller: myController
        ),
        Text(data.myString),
        ElevatedButton(onPressed: () => data.changeMyString(myController.text), child: const Text("텍스트 바꾸기"))
      ],
    );
  }
}
```
위 코드는 setter와 거의 비슷하다. 하지만 set으로는 동작할 수 없다. 상태가 바뀔 때마다 그 값을 변화시켜주니 setState와 유사하다고 볼 수도 있다.

---


