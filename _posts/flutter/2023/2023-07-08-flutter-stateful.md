---
title: Flutter - StatefulWidget
date: 2023-07-08 10:29:57 +/-09:00
category: [flutter/basic]
tag: [flutter, stateful]
---

[Do it! 깡샘의 플러터 & 다트 프로그래밍](https://www.yes24.com/Product/Goods/117206541)

본 내용은 위 책을 많이 참고했다. 상세히 공부하고 싶은 사람은 책을 참조 바란다.

이번에는 조금 긴 코드를 가져올 것이다. 단번에 이해가 되지 않으면 정상이니까 너무 우려할 필요는 없다.
```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Stateful 앱입니다."),
        ),
        body: Widget1(),
      )
    );
  }
}

class Widget1 extends StatefulWidget {
  @override
  State createState() {
    return MyStatefulWidget();
  }
}

class MyStatefulWidget extends State {
  bool clickCheck = false;
  String showText = "체크했나요?";

  void clicked () {
    setState(() {
      if (clickCheck) {
        showText = "체크하지 않았어요!";
        clickCheck = !clickCheck;
      }
      else {
        showText = "체크했어요!";
        clickCheck = !clickCheck;
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Center(
    child: Row(
      children: [
        IconButton (
          icon: (clickCheck
    ? Icon(Icons.radio_button_on, size: 30)
            : Icon(Icons.radio_button_off, size: 30)),
    color: Colors.indigo[200],
    onPressed: clicked,
    ),
    Container(
    padding: EdgeInsets.all(20.0),
    child: Text("$showText", style: TextStyle(fontSize: 50),
          )
        )
      ]
    ),
    );
  }
}
```

너무 길어서 정신이 없다. 이제 우리는 이것을 나눠서 풀어보자.

```dart
void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Stateful 앱입니다."),
        ),
        body: Widget1(),
      )
    );
  }
}

class Widget1 extends StatefulWidget {
  @override
  State createState() {
    return MyStatefulWidget();
  }
}
```

여기까지는 이해가 쉽다. MyApp을 실행하는 건 기본이니 넘어가고, MyApp은 StatelessWidget을 상속받는다. 그렇다. 기본 화면은 결코 변하지 않기에 StatelessWidget을 상속받는 것이다. Stateful은 매번 변동 사항을 새로이 갱신하는데, Widget은 **불변 객체**다. 다시 말해 하나의 요소만 변동되더라도 컴파일러가 전체를 새로이 바꿔버려야 하는데 이는 너무 비효율적이다. 겉으로 보기에는 아무것도 아닐 수 있어도 내부적으로는 정말 많은 동작이 따르기 때문이다. 따라서 우리는 바꿀 부분만 한정해서 StatefulWidget으로 지정하는 것이다.

아무튼, 우리는 Widget1() 함수를 실행시킬 것이다. 이것은 MyApp과 달리 StatefulWidget을 상속받는다. 즉, 동적이다.

상태창을 만들고 이것 또한 새로운 클래스를 반환한다.
```dart
class MyStatefulWidget extends State {
  bool clickCheck = false;
  String showText = "체크했나요?";

  void clicked () {
    setState(() {
      if (clickCheck) {
        showText = "체크하지 않았어요!";
        clickCheck = !clickCheck;
      }
      else {
        showText = "체크했어요!";
        clickCheck = !clickCheck;
      }
    });
  }
```

bool clickCheck는 클릭을 체크한다. 클릭이 확인될 때마다 텍스트를 바꿔준다. 즉,

clicked() 함수가 실행될 때마다 우리는 showText의 내용을 바꿀 것이다. 일단 디폴드 값은 String showText에 나온 그대로다.

clickCheck에 True와 False가 아닌, 반대 연산자를 사용하여, True일 경우 False를, False일 경우 True를 대입해준다. 말 그대로 스위치를 누르는 것이다.
```dart
@override
  Widget build(BuildContext context) {
    return Center(
    child: Row(
      children: [
        IconButton (
          icon: (clickCheck
    ? Icon(Icons.radio_button_on, size: 30)
            : Icon(Icons.radio_button_off, size: 30)),
    color: Colors.indigo[200],
    onPressed: clicked,
    ),
    Container(
    padding: EdgeInsets.all(20.0),
    child: Text("$showText", style: TextStyle(fontSize: 50),
          )
        )
      ]
    ),
    );
  }
}
```
센터에 설정했지만 Row를 부여하여 가로로 정렬한다. 2개를 나열할 테니 children을 사용하고 리스트를 넣는다.

리스트의 첫번째 요소 IconButton에서 icon을 가져온다. icon은 clickCheck에 의한 삼항연산자로 반응한다. 이래서 아까 우리가 clickCheck를 bool 타입으로 설정한 것이다.

True일 경우 라디오 버튼을 키고, False일 경우 라디오 버튼을 끈다.

색깔은 남색으로 넣었고, onPressed는 누를 경우 반응하는 거니까 함수 clicked를 넣는다. 주의할 점은 ()를 붙이지 않는다. 함수 실행이 아니라 어떤 함수를 킬 것인가를 보는 것이니까 말이다.

리스트의 두번째 요소 컨테이너는 텍스트를 표기해준다.

여기까지 읽어봤다면 리액트, 뷰 등의 JS 프레임워크와 매우 유사하다는 것을 알 수가 있다.

---