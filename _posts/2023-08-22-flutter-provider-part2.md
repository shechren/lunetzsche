---
title: Flutter - Provider (2)
date: 2023-08-22 23:35:39 +/-09:00
category: [flutter/state]
tag: [flutter, state, provider]
---

MultiProvider는 하나의 위젯에 여러개의 프로바이더를 받을 수 있다.


```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      body: MultiProvider(
        providers: [
          ChangeNotifierProvider<MyString1>(
            create: (context) => MyString1(),
          ),
          ChangeNotifierProvider<MyString2>(
            create: (context) => MyString2(),
          ),
          ChangeNotifierProvider<MyInt>(
            create: (context) => MyInt(),
          ),
        ],
        child: MyWidget(),
      ),
    ));
  }
}

class MyString1 with ChangeNotifier {
  String _myString = "";

  String get myString => _myString;

  changeMyString(value) {
    _myString = value;
    notifyListeners();
  }
}

class MyString2 with ChangeNotifier {
  String _myString = "";

  String get myString => _myString;

  changeMyString(value) {
    _myString = value;
    notifyListeners();
  }
}

class MyInt with ChangeNotifier {
  int _myInt = 0;

  int get myInt => _myInt;

  changeMyInt(value) {
    _myInt = value;
    notifyListeners();
  }
}

class MyWidget extends HookWidget {
  MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final data1 = Provider.of<MyString1>(context);
    final data2 = Provider.of<MyString2>(context);
    final data3 = Provider.of<MyInt>(context);
    final myController1 = useTextEditingController();
    final myController2 = useTextEditingController();
    final myController3 = useTextEditingController();

    return Column(
      children: [
        TextField(controller: myController1),
        Text(data1.myString),
        TextField(controller: myController2),
        Text(data2.myString),
        TextField(controller: myController3,keyboardType: TextInputType.number,),
        Text(data3.myInt.toString()),
        ElevatedButton(
            onPressed: () {
              data1.changeMyString(myController1.text);
              data2.changeMyString(myController2.text);
              data3.changeMyInt(int.parse(myController3.text));
            },
            child: const Text("Provider 바꾸기"))
      ],
    );
  }
}
```

위는 String 2개와 int 1개의 상태를 바꾸는 프로바이더를 만든 것이다.
context는 위치를 나타낸다. 즉, 우리는 context를 통해 다른 클래스에 접근할 수 있다.

---

ProxyProvider는 다른 Provider를 의존(dependency)할 때 사용한다. ProxyProvider는 제네릭이 2개 필요한데, \<A, B\>에서 A는 전달받을 값. 그러니까 우리가 언제나 선언하는 Provider다. B는 그 값을 참조해서 만들 상태의 타입이다.
A는 여러개가 들어갈 수 있다. 리스트로 들어가는 건 아니고, 그냥 쉼표로 하나씩 늘어나는 것이다. 최대 6개까지 가능하다. 늘릴 적에는 Provider 뒤에 2~6까지 숫자를 붙인다.

ProxyProvider는 update를 작성해야 하는데, update에서는 3개의 인자를 받는다. 맨 앞은 BuildContext context이고, 맨 끝은 마지막에 만들어지는 상태다. 즉, 가운데에 들어가는 인자가 참조되는 상태가 된다. 왜 2번째 인자라고 말하지 않냐면, 위에서 말했듯이 A가 여러개가 들어가면 가운데에 들어가는 인자의 수도 하나씩 늘어나게 되는 것이다.

---

