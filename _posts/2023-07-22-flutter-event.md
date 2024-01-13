---
title: Flutter - 이벤트(event)
date: 2023-07-22 11:00:35 +/-09:00
category: [flutter/basic]
tag: [flutter, event]
---

[Do it! 깡샘의 플러터 & 다트 프로그래밍](https://www.yes24.com/Product/Goods/117206541)

본 내용은 위 서적을 많이 참고했다. 상세한 내용은 해당 링크를 참조 바란다.

IconButton, ElevatedButton, FloatingActionButton 같은 위젯들에서도 이벤트 처리가 가능하다. 그런데 그 모든 이벤트들 역시 내부적으로는 GestureDetector를 이용한다. 따라서 우리는 GestureDetector에 대해 반드시 숙지하고 넘어갈 필요성이 있다.
```dart
class HomeView extends StatelessWidget {
  const HomeView({super.key});

  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      child: Image.asset('images/1.png', width: 400, height: 400),
      onTap: () {
        print("이미지를 클릭했어요!");
      });
  }
}
```
위의 HomeView 클래스에서는 1.png를 만든 뒤 그것을 클릭하면 print 문을 반환한다.

버튼에서 이벤트를 발생시키는 건 내장 메소드를 쓰면 편하다. ElevatedButton은 심플한 인터페이스와 깔끔한 동작을 자랑한다.
```dart
class MyEvent extends StatelessWidget {
  const MyEvent({super.key});

  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: ElevatedButton(onPressed: onPressed, child: child)
    );
  }
}
```

ElevatedButton은 2개의 프로퍼티를 메소드로 받는다. onPressed와 child가 그것이다.

onPressed는 함수를 리턴한다. 말 그대로 버튼을 누르면 발생하는 이벤트다.

child는 버튼의 속성이다.
```dart
class MyEvent extends StatelessWidget {
  const MyEvent({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ElevatedButton(onPressed: () {
        print ('버튼을 눌렀어요!');
      }, child: Text("버튼 속성", style: TextStyle(color: Colors.white)),
          style: ButtonStyle(backgroundColor: MaterialStateProperty.all(Colors.green)))
    );
  }
}
```
버튼 이벤트는 print 문을 호출하게 만들었다.

버튼 속성으로는 버튼 배경색으로 초록색, 내부 텍스트는 하얀색으로 만들어보았다.

---