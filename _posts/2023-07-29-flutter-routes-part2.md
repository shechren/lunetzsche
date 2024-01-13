---
title: Flutter - Routes(2)
date: 2023-07-29 10:00:29 +/-09:00
category: [flutter/basic]
tag: [flutter, routes]
---

```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';

void main() {
  runApp(const MyApp());
}

appBarClass() {
  return AppBar(
    title: const Text("Home Page"),
  );
}

final GoRouter _router = GoRouter(
    routes: [
      GoRoute(
        path: '/',
        builder: (context, state) => const Home(),
      ),
      GoRoute(
        path: '/FirstScreen',
        builder: (context, state) => const FirstScreen(),
      ),
      GoRoute(
        path: '/SecondScreen',
        builder: (context, state) => const SecondScreen(),
      ),
    ]
);


class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp.router(
      routerConfig: _router,
      title: "Go_Router Test",
    );
  }
}


class Home extends StatelessWidget {
  const Home({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: appBarClass(),
      body: Center(
        child: Column(
          children: [
            ElevatedButton(
                onPressed: () => context.go('/FirstScreen'),
                child: const Text('FirstScreen')),
            ElevatedButton(
                onPressed: () => context.push('/SecondScreen'),
                child: const Text('SecondScreen')),
          ],
        )
      )
    );
  }
}

class FirstScreen extends StatelessWidget {
  const FirstScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: appBarClass(),
      body: Center(
        child: Column(
          children: [
            ElevatedButton(
                onPressed: () => context.go('/'),
                child: const Text("Return"))
          ],
        )
      )
    );
  }
}

class SecondScreen extends StatelessWidget {
  const SecondScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        children: [
          ElevatedButton(
              onPressed: () => context.pop(),
              child: const Text("Return"))
        ],
      )
    );
  }
}
```
코드가 길지만 최대한 간략히 설명하자면,

go_routes 라이브러리를 이용하여 만들었다.

FirstScreen은 context.go를 이용했고,

SecondScreen은 context.push를 이용했다.

go는 말 그대로 현재 페이지를 전환하는 것이다. 따라서 페이지 자체가 바뀌게 된다.

반면 push는 스택 구조 위에 한 단계씩 페이지를 쌓는 것이다. 따라서 화면을 덮게 된다.

뒤로 갈 경우 go는 go로 돌아가는 것이 맞고,

반면 push는 기존 페이지를 지워야 하니 pop을 이용해서 돌아간다.

---
  
