---
title: Flutter - Material Theme
date: 2023-07-09 22:43:14 +/-09:00
category: [flutter/basic]
tag: [flutter, material]
---

구글의 머티리얼 디자인을 따르면 마치 어도비 컬러, 구글 컬러를 이용하는 듯하게 테마 색상을 설정할 수 있다.

색상을 고르기 어려울 경우 이 색상 위주로 디자인의 통일성을 지킬 수 있게 된다.
```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        debugShowCheckedModeBanner: false,
        title: "MyApp",
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
          ),
      home: const Home(),
    );
  }
}

// 중략

Container (
  width: 100,
  height: 100,
  color: Theme.of(context).colorScheme.primary,
  child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
    overflow: TextOverflow.ellipsis,
    maxLines: 4,
    style: TextStyle(fontSize: 15),
  ),
),
```
사용할 때는 Theme.of(context).colorScheme.??? 이다.

primary, secondary, tertiary 등 다양한 커스텀이 가능하다.

---