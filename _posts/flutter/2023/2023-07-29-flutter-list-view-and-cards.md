---
title: Flutter - List View & Cards
date: 2023-07-29 10:57:14 +/-09:00
category: [flutter/basic]
tag: [flutter, list view, card]
---

```dart
mainPage() {
  return ListView(
    padding: const EdgeInsets.all(10),
    children: [
      cards("카드"),
      cards("카드2"),
      cards("카드3"),
    ],
  );
}
```
리스트 뷰에 카드를 추가하려 한다. cards 함수는 String 하나를 받는다.
```dart
cards(String desc) {
  return Card(
    color: Colors.grey,
    child: SizedBox(
      width: 250,
      height: 250,
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Icon(
            Icons.add,
          ),
          Text(desc),
        ],
      )
    )
  );
}
```
Icon은 더하기 아이콘이고, Text로 아까 넘긴 String을 받는다.

![flutter-list-view-and-cards.png](/assets/postingImage/flutter-list-view-and-cards.png)

---