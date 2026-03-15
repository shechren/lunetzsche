---
title: Flutter - 상태 관리
date: 2023-08-06 19:37:00 +/-09:00
category: [flutter/state]
tag: [flutter, state, stateful]
---

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

void main() {
  runApp(const Home());
}


class Home extends StatefulWidget {
  const Home({super.key});

  @override
  State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
  bool isHeart = true;

  void _isHeartCalled() {
    isHeart = !isHeart;
  }

  final Icon _heartIcon = const Icon(FontAwesomeIcons.solidHeart, color: Colors.red, size: 50);
  final Icon _heartBlankIcon = const Icon(FontAwesomeIcons.heart, color: Colors.red, size: 50);


  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: SafeArea(
              child: SizedBox(
                width: 400,
                height: 400,
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    GestureDetector(
                      child: isHeart ? _heartIcon : _heartBlankIcon,
                      onTap: () {
                        setState(() {_isHeartCalled();}
                        );
                      },
                    )
                  ],
                ),
              ),
            )
        )
    );
  }
}
```
하트를 누를 때마다 빈 하트와 채워진 하트가 왔다갔다한다.
이러면 마음이 뭔가 편안하다 싶겠지만 아니다. 왜냐면 마테리얼 박스 이하의 모든 것이 StateFul에 다 들어가 있기 때문이다.
그럼 우리는 Column을 따로 분리해서 관리해야 한다.

---

우리가 할 일은 마치 추상 클래스를 선언하듯이, StatelessWidget에서 final로 선언만 해두고, StatefulWidget에서 상태를 변경하도록 넘기는 것이다.
즉, 선언 부분과 실행 부분을 별도로 분리하는 것이다.

```dart
import 'package:flutter/material.dart';
import 'package:font_awesome_flutter/font_awesome_flutter.dart';

void main() {
  runApp(const Home());
}


class Home extends StatefulWidget {
  const Home({super.key});


  @override
  State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
  // isHeart 기본값 = true
  bool _isHeart = true;

  
  void ChangeState() {
    setState(() {
      // heartChanged 역할은 bool을 반전시키는 것
      _isHeart = !_isHeart;
    });
  }


  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: SafeArea(
              child: SizedBox(
                width: 400,
                height: 400,
                // myIcon을 불러올 적에 isHeart와 heartChanged의 값을 지정한다.
                child: myIcon(isHeart: _isHeart, heartChanged: ChangeState),
              ),
            )
        )
    );
  }
}
class myIcon extends StatelessWidget {
  final bool isHeart; // callback Property
  final VoidCallback heartChanged; // callback Function

  const myIcon({
    // isHeart와 heartChanged -> required
    required this.isHeart,
    required this.heartChanged,
    Key? key,
  }) : super(key: key);

  final Icon _heartIcon = const Icon(FontAwesomeIcons.solidHeart, color: Colors.red, size: 50);
  final Icon _heartBlankIcon = const Icon(FontAwesomeIcons.heart, color: Colors.red, size: 50);

  @override
  Widget build(BuildContext context) {

    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        GestureDetector(
          onTap: heartChanged,
          child: isHeart ? _heartIcon : _heartBlankIcon),
      ],
    );
  }
}
```

[전체 코드](https://github.com/shechren/DIM/blob/master/do12/lib/main.dart)

---