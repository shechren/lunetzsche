---
title: Flutter - Provider (3)
date: 2023-08-27 19:40:21 +/-09:00
category: [flutter/state]
tag: [flutter, state, provider]
---

오늘은 **Consumer**에 대해 공부할 것이다.

```dart
class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            body: ChangeNotifierProvider<MyString>(
                create: (context) => MyString(), child: MyWidget())));
  }
}

class MyString with ChangeNotifier {
  String _myString = "";

  String get myString => _myString;

  changeMyString(value) {
    _myString = value;
    notifyListeners();
  }
}

class MyWidget extends HookWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final useFormKey = useMemoized(() => GlobalKey<FormState>());
    var myString = Provider.of<MyString>(context);
    print('my widget');

    return Column(
      children: [
        Form(
          key: useFormKey,
          child: TextFormField(
            onSaved: (String? value) {
              myString.changeMyString(value);
            },
          ),
        ),
        ElevatedButton(
            onPressed: () {
              if (useFormKey.currentState != null)
                {
                  useFormKey.currentState!.save();
                  print(myString.myString);
                }
            }, child: const Text("출력")),
        AnotherWidget(),
      ],
    );
  }
}

class AnotherWidget extends HookWidget {
  AnotherWidget({super.key});

  @override
  Widget build(BuildContext context) {
    print('another widget');
    return Text("AnotherWidget");
  }
}
```

AnotherWidget은 MyWidget 안에서 실행되었는데, 문제는 Provider를 쓰지 않았음에도 계속해서 re-build가 됨을 확인할 수 있다.
이것은 매우 큰 낭비다.

```dart
class MyWidget extends HookWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    print('my widget');

    return Column(
      children: [
        Consumer<MyString>(
          builder: (BuildContext context, value, Widget? child) {
            return MyColumn();
          },
        ),
        AnotherWidget(),
      ],
    );
  }
}

class AnotherWidget extends HookWidget {
  AnotherWidget({super.key});

  @override
  Widget build(BuildContext context) {
    print('another widget');
    return Text("AnotherWidget");
  }
}

class MyColumn extends HookWidget {
  MyColumn({super.key});

  @override
  Widget build(BuildContext context) {
    final useFormKey = useMemoized(() => GlobalKey<FormState>());
    var myString = Provider.of<MyString>(context);

    return Column(children: [
      Form(
        key: useFormKey,
        child: TextFormField(
          onSaved: (String? value) {
            myString.changeMyString(value);
          },
        ),
      ),
      ElevatedButton(
          onPressed: () {
            if (useFormKey.currentState != null) {
              useFormKey.currentState!.save();
              print(myString.myString);
            }
          },
          child: const Text("출력")),
    ]);
  }
}
```
Consumer는 위와 같이 사용한다. Provider와 마찬가지로 인자가 여러가지면 Consumer 뒤에 2~6까지의 수를 붙이고 **가운데**의 인자 수를 하나씩 늘리면 된다.
희한하게도 Consumer의 인자 수는 앞서 말했듯이 가운데, 그러니까 (context, value, child) 여기서 value가 바로 인자다.

Consumer를 사용하면 말 그대로 프로바이더 연결을 혼자 그 구간에서 독식해버린다. 따라서 MyColumn은 실행되지만 AnotherWidget이 re-build되지는 않는다.

---