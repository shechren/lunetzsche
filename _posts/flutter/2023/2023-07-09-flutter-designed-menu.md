---
title: Flutter - 메뉴 만들기
date: 2023-07-09 16:07:38 +/-09:00
category: [flutter/basic]
tag: [flutter, menu]
---

![flutter-designed-menu.png](/assets/postingImage/flutter-designed-menu.png)

[https://github.com/shechren/DIM/tree/master/Do2](https://github.com/shechren/DIM/tree/master/Do2)

전체 코드는 여깄다.

여기서도 또한 머티리얼 앱을 만들어두고 시작한다.
```dart
    debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(
          title: Text("메인화면"),
        ),
        drawer: Drawer(
          width: 150,
          child: ListView(
            children: const [
              ListTile(title: Text("취업")),
              ListTile(title: Text("문화")),
              ListTile(title: Text("자립준비")),
              ListTile(title: Text("고립은둔")),
              ListTile(title: Text("센터목록")),
            ],
          ),
        ),
```
drawer를 만들면 appBar 좌측에 메뉴가 추가된다.

폭을 150으로 하고, children으로 5개의 요소를 추가해본다.

```dart
        bottomNavigationBar: BottomNavigationBar(
          items: const [
            BottomNavigationBarItem(
              label: "show",
              icon: Icon(Icons.search, color: Colors.indigo),
            ),
            BottomNavigationBarItem(
              label: "add",
              icon: Icon(Icons.add, color: Colors.amberAccent),
            ),
            BottomNavigationBarItem(
              label: "settings",
              icon: Icon(Icons.settings, color: Colors.black45),
            ),
        ])
      ),
```
바닥에 내비게이션 바를 만든다. show와 add, settings를 추가한다. 아이콘을 추가하고 컬러를 바꿔본다.

---