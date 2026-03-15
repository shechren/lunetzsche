---
title: Flutter - Axis
date: 2023-08-04 18:49:28 +/-09:00
category: [flutter/basic]
tag: [flutter, axis, row, column]
---

Axis는 Row와 Column의 파라미터다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Home()
    );
  }
}

class Home extends StatelessWidget {
  const Home({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Row(
        // axis
        children: [
          Container(
            color: Colors.red,
            width: 50,
            height: 50,
          ),
          Container(
            color: Colors.yellow,
            width: 50,
            height: 50,
          ),
          Container(
            color: Colors.green,
            width: 50,
            height: 50,
          ),
        ],
      ),
    );
  }
}
```

이 위의 주석 부분에 코드를 적으면 된다.
참고로 Row라고 적혀있으니 모두 가로로 정렬될 것이며,
Column으로 하면 세로로 정렬될 것이다.

**mainAxisAlignment**: 중심축을 기반으로 정렬한다.
 * MainAxisAlignment.start: 시작 지점부터 일렬로 나열한다.
 * MainAxisAlignment.center: 중간 부분에 일렬로 나열한다.
 * MainAxisAlignment.end: 마지막 부분부터 일렬로 나열한다.
 * MainAxisAlignment.spaceBetween: 양쪽 끝을 사이드에 붙이고 동일한 간격으로 나열한다.
 * MainAxisAlignment.spaceEvenly: 모든 간격을 균등하게 하고 시작 부분과 끝부분의 간격도 동일하다.
 * MainAxisAlignment.spaceAround: 모든 간격을 균등하게 하고 시작 부분과 끝부분의 간격은 다른 아이템과의 간격의 1/2로 한다.

**CrossAxisAlignment**: 상대축을 중심으로 정렬한다. 즉, Row라면 세로축, Column이라면 가로축이다.
* CrossAxisAlignment.start: 상대축 시작 지점부터 일렬로 나열한다.
* CrossAxisAlignment.center: 상대축 중간 부분에 일렬로 나열한다.
* CrossAxisAlignment.end: 상대축 마지막 부분부터 일렬로 나열한다.
* CrossAxisAlignment.stretch: 상대축 길이를 최대(상위 위젯) 범위로 늘린다.

**mainAxisSize**: 중심축을 기반으로 범위를 지정한다.
* MainAxisSize.max: 사이드 부분을 완전히 채운다.
* MainAxisSize.min: 가진 아이템의 범위만큼만 채운다.

---