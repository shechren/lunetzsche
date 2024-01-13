---
title: Flutter - bloc (2)
date: 2023-08-22 18:54:20 +/-09:00
category: [flutter/state]
tag: [flutter, state, bloc]
---

bloc을 이용하여 디자인을 해볼 시간이다.
카운터부터 만들어보자.


```dart
abstract class Counter {
  int count;
  Counter(this.count);
}

class Increase extends Counter {
  Increase(count) : super(count);
}

class Decrease extends Counter {
  Decrease(count) : super(count);
}
```

추상 클래스로 더하기와 빼기를 만들어준다. 당연히 여기서는 아무것도 정의하지 않는다.


```dart
class BlocCounter extends Bloc<Counter, int> {
  BlocCounter() : super(0) {
    on<Increase>((event, emit) => emit(state + event.count));
    on<Decrease>((event, emit) => emit(state - event.count));
  }

  @override
  void onEvent(Counter event) {
    super.onEvent(event);
  }

  @override
  void onTransition(Transition<Counter, int> transition) {
    super.onTransition(transition);
    print(transition);
  }
}
```

Bloc은 2개의 제네릭을 받는다. 하나는 우리가 받을 형식이고, 또하나는 그 형식의 타입이다.
우리는 Counter 클래스를 형식으로 받을 것이고, 그것은 int 타입이다.
생성자 함수를 통해 on\<제네릭\>(함수)를 선언하는데 이러한 패턴이 바로 **Business Logic Component, 줄여서 BLoC** 패턴이다.
하지만 너무 많은 생성자 함수를 추가하는 것은 SRP(Single Responsibility Principle)를 위반하게 되기에 주의해야 한다.

 * SRP(Single Responsibility Principle)란 하나의 클래스는 하나의 책임만 가지도록 설계되어야 한다는 소프트웨어 디자인 원칙이다.

on\<제네릭\>에서 우리가 실행할 클래스를 함수처럼 넘겨받고, 그것의 명령을 설계해준다.
이제 마지막으로 구동되는 부분을 작성할 시간이다.

```dart
class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          body: BlocProvider<BlocCounter>(
        create: (context) => BlocCounter(),
        child: const BuildWidget(),
      )),
    );
  }
}
```
기본적으로 bloc 또한 provider를 이용하기에 프로바이더와 유사한 패턴을 가진다.

```dart
class BuildWidget extends HookWidget {
  const BuildWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final counter = BlocProvider.of<BlocCounter>(context);
    return BlocBuilder<BlocCounter, int>(builder: (context, count) {
      return Center(
        child: Column(
          children: [
            Text('${counter.state}', style: const TextStyle(fontSize: 30)),
            GestureDetector(
              onTap: () => counter.add(Increase(1)),
              child: Container(
                width: 50,
                height: 50,
                decoration: BoxDecoration(
                  color: Colors.green,
                  borderRadius: BorderRadius.circular(50),
                ),
                child: const Icon(Icons.exposure_plus_1),
              ),
            ),
            GestureDetector(
              onTap: () => counter.add(Decrease(1)),
              child: Container(
                width: 50,
                height: 50,
                decoration: BoxDecoration(
                  color: Colors.blue,
                  borderRadius: BorderRadius.circular(50),
                ),
                child: const Icon(Icons.exposure_minus_1),
              ),
            ),
          ],
        ),
      );
    });
  }
}
```
일급 함수 counter에서 of를 받아주고, BlocCounter 제네릭을 지정해준다. 우리는 BlocCounter 형식을 넘겼으니까 BlocCounter 형식을 받아야 한다.
builder 함수를 호출하고, 리턴 값으로 우리의 위젯들 안에 counter의 메소드를 넣으면 완성된다.

---