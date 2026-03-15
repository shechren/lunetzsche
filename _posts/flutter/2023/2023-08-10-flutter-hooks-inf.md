---
title: Flutter - Hooks 소개
date: 2023-08-10 10:17:46 +/-09:00
category: [flutter/state]
tag: [flutter, state, hooks]
---

```dart
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    final useController = useState(0);

    return MaterialApp(
        home: Scaffold(
          body: Text(
            useController.value.toString(),
            style: const TextStyle(fontSize: 60),
          ),
          floatingActionButton: FloatingActionButton(
            child: const Icon(Icons.add),
            onPressed: () {
              useController.value++;
          }),
    ));
  }
}
```

지금껏 JavaScript라는 선대가 싸놓은 똥을 먹고 살아온 우리에게 Flutter는 얼마나 고귀한 경험을 선물로 주셨는지 모른다. 그것에 더해 Hooks이라는 라이브러리가 추가되었다. 처음에는 "아 이것도 JS처럼 개떡같이 되어가는 구나" 했으나 실상은 아니었다.

위의 코드가 보이는가? Stateful과 Stateless가 합쳐져 단 하나로 모든 상태관리가 가능하다. 이 선에서 너무 눈물나게 간결하고 행복하다.

텍스트를 입력하면 프린트로 받기 위해선 기존에는 별별 단계를 거쳐야 했지만 훅은 하나면 충분하다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends HookWidget {
  MyApp({super.key});
  String? text;

  @override
  Widget build(BuildContext context) {
    final myController = useTextEditingController(text: text);

    return MaterialApp(
        home: Scaffold(
          body: Column(
            children: [
              TextField(
                controller: myController,
              ),
              ElevatedButton(onPressed: () =>
                  print (myController.text), child: Text("텍스트"))
            ],
          )
        )
    );
  }
}
```

---