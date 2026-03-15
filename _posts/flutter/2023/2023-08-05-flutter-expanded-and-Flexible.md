---
title: Flutter - Expanded와 Flexible
date: 2023-08-05 13:04:15 +/-09:00
category: [flutter/basic]
tag: [flutter, row, column, expanded, flexible]
---

```dart
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

우리는 이 코드를 기준으로 조금씩 변형시킬 것이다.
컨테이너 위에 마우스를 대고 우클릭 - Show Context Action 또는 Alt Enter를 누르고, Wrap with widget을 연다.

```dart
Expanded(
  child: Container(
    color: Colors.green,
    width: 50,
    height: 50,
  ),
),

Expanded로 컨테이너를 감싼다. 우리는 그린에 감쌌는데, 이러면 다른 컨테이너는 크기가 그대로이며 그린만 남은 모든 Row 공간을 채우게 된다.
Yellow도 같은 방법으로 감싸면 이번에는 그린과 옐로가 남은 공간을 채우는데, 그린과 옐로의 비율은 1:1이다.
마지막으로 Red도 감싸게 되면 1:1:1 비율로 남은 모든 공간이 채워지게 된다.


```dart
Flexible(
  child: Container(
    color: Colors.green,
    width: 50,
    height: 50,
  ),
),
```
이번에는 그린 컨테이너의 Expanded의 위젯을 Flexible로 감쌌다. 옐로는 전과 마찬가지의 공간을 차지하지만, 그린은 레드와 똑같이 최솟값만 유지한다.

즉, Expanded는 **최댓값**을 차지하고, Flexible은 **최솟값**을 차지한다.
Expanded 요소가 많을 경우 **그 크기는 1/n**하게 된다.
Expanded와 Flexible은 **Row와 Column 내에서만 동작**한다.

--