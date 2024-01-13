---
title: Flutter - Cubit (1)
date: 2023-08-24 20:35:15 +/-09:00
category: [flutter/state]
tag: [flutter, state, cubit]
---

Cubit은 Bloc의 하위 개념이다. 블록은 블록 내의 이벤트를 거쳐 서로간의 정보를 전달하는 반면, 큐빗은 함수를 직접 호출하기에 보다 다이렉트한 연결이 가능하다. 무엇을 쓸지 고민이 된다면 우선 큐빗을 쓰고 향후 블록이 필요할 경우 블록으로 리팩토링하는 작업이 좋다고 한다.

또한 Cubit의 경우 onTransition함수가 없기에 블록처럼 상태 감시를 할 수 없다는 점도 유의해야 한다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:flutter_hooks/flutter_hooks.dart';

void main() {
  runApp(MyApp());
}

class MyCubit extends Cubit<int> {
  MyCubit() : super(0);

  void increase() => emit(state + 1);
  void decrease() => emit(state - 1);

  @override
  void onChange(Change<int> change) {
    super.onChange(change);
  }
}

class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => MyCubit(),
      child: const MyWidget(),
    );
  }
}

class MyWidget extends HookWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    var cubit = BlocProvider.of<MyCubit>(context);
    return MaterialApp(
      home: Scaffold(
        body: BlocBuilder<MyCubit, int>(
          builder: (context, count) {
            return Column(
              children: [
                Text(count.toString()),
                Row(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    ElevatedButton(onPressed: cubit.increase, child: const Text("더하기")),
                    ElevatedButton(onPressed: cubit.decrease, child: const Text("빼기"))
                  ],
                )
              ],
            );
          }
        ),
      ),
    );
  }
}
```

블록에 비해 굉장히 간단해졌다. 추상 클래스를 선언하고, 생성자 안에 생성자 함수를 선언해야 했던 골칫거리 Bloc에 비해 큐빗은 그냥 함수를 직접 만들어서 직접 불러버리면 그만이다.
기본적인 사용법은 같고 상속받는 클래스가 Bloc이 아닌 Cubit인 것이 차이점이다.
그리고 인스턴스화를 한 다음 함수에 직접 접근하면 그걸로 끝이다.

---