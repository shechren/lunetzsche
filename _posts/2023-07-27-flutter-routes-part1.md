---
title: Flutter - Routes(1)
date: 2023-07-27 14:11:51 +/-09:00
category: [flutter/basic]
tag: [flutter, routes]
---

화면을 이동하는 Routes에 대해 익힐 시간이다.
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        initialRoute: '/',
        routes: routes,
    );
  }
}
```

Material App 안에 initialRoute와 routes를 설정한다. routes에 대해서는 나중에 다른 파일로 빼서 설정할 것이다.

참고로 home을 설정하면 initialRoute와 충돌하니 주의해야 한다.
```dart
class Home extends StatelessWidget {
  const Home({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Column(
          children: [
            ElevatedButton(
              onPressed: () {Navigator.pushNamed(context, '/first');
              },
              child: const Text("Button1"),
            ),
            ElevatedButton(
              onPressed: () {Navigator.pushNamed(context, '/second');
              },
              child: const Text("Button2"),
            ),
          ],
        )
    );
  }
}

class FirstScreen extends StatelessWidget {
  const FirstScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        width: 400,
        height: 400,
        decoration: BoxDecoration(
          border: Border.all(width: 5, color: Colors.indigo)
        ),
      ),
    );
  }
}

class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        width: 200,
        height: 200,
        decoration: BoxDecoration(
            border: Border.all(width: 5, color: Colors.green)
        ),
      ),
    );
  }
}
```

홈과 2개의 스크린을 Scaffold로 설정한다. 홈에 들어갈 화면이다.

홈에서는 2개의 스크린으로 이동시키는 ElevatedButton을 만들어준다.

Navigator 탭의 pushNamed 함수를 호출한다. 이 함수는 두 개의 값을 받는다.

하나는 context를 받는데 우리가 위젯을 선언할 때의 그 context다.

두 번째는 라우트로 쓸 String 값을 받는다.
```dart
import 'package:flutter/material.dart';
import 'main.dart';

final Map<String, WidgetBuilder> routes = {
  '/': (context) => const Home(),
  '/first': (context) => const FirstScreen(),
  '/second': (context) => const SecondScreen(),
};
```
final Map&lt;String, WidgetBuilder&gt; routes = 를 선언해준다. 아까 설명하겠다고 한 그 routes다. 다른 파일에 만든 것이다.

그리고 우리가 연결할 라우트를 등록해준다.

마지막으로 메인에서 라우트 파일을 불러오기하면 등록이 끝난다.

[https://github.com/shechren/DIM/tree/master/do8](https://github.com/shechren/DIM/tree/master/do8)

---