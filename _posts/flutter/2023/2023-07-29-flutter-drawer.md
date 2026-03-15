---
title: Flutter - Drawer
date: 2023-07-29 11:41:23 +/-09:00
category: [flutter/basic]
tag: [flutter, drawer]
---

```dart
myDrawer() {
  return Align(
    alignment: Alignment.topLeft,
    child: SizedBox(
      height: 600,
      child: Drawer(
        child: ListView(
          padding: const EdgeInsets.all(10),
          children: [
            ListTile(
              leading: Icon(Icons.search),
              title: Text("검색"),
              onTap: () {

              },
            ),
            ListTile(
              leading: Icon(Icons.search),
              title: Text("취업"),
              onTap: () {

              },
            ),
            ListTile(
              leading: Icon(Icons.search),
              title: Text("자립준비"),
              onTap: () {

              },
            ),
            ListTile(
              leading: Icon(Icons.search),
              title: Text("고립은둔"),
              onTap: () {

              },
            ),
            ListTile(
              leading: Icon(Icons.search),
              title: Text("센터목록"),
              onTap: () {

              },
            ),
          ],
        )
      ),
    )
  );
}
```
Drawer 자체의 높이를 조절하기 위해 Container(SizedBox)로 감싸고,

그것을 재차 Align을 조정하여 topLeft에 박아두었다.

ListView로 내 아이템을 정렬하면 되며,

padding으로 ListTile 사이의 간격을 조절할 수 있다.

---