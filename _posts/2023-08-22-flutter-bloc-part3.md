---
title: Flutter - bloc (3)
date: 2023-08-22 20:05:06 +/-09:00
category: [flutter/state]
tag: [flutter, state, bloc]
---

BlocObserver는 말 그대로 블록 관찰자다. 블록의 상태 변화를 감시할 수 있다.
왜 이런 걸 쓰냐 할 수도 있지만 debug 과정에서 우리는 수많은 난관을 마주하게 된다. 그런데 그때마다 전후로 print문을 붙여가며 하는 건 생산성에 있어 너무나도 큰 낭비다. 모든 블록들을 찾아다니는 것도 일이기도 하다.
그렇기에 BlocObserver는 이러한 노력을 줄여주는 역할을 한다. 개발상의 편의인 것이다.
BlocObserver는 모든 블록을 통제할 수 있기에 매번 블록들을 찾아다니지 않고도 하나의 파일에서 전부 감시할 수 있게 된다.

방법은 BlocObserver를 선언하고 그것을 메인 함수에 등록시키면 된다.
참고로 BlocOverrides.runZoned()는 더이상 지원되지 않는 메소드이기에 유의해야 한다.

```dart
import 'package:flutter_bloc/flutter_bloc.dart';
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';

void main() {
  Bloc.observer = BlocObs();
  //BlocOverrides.runZoned()는 v.9.0.0에서 제거됨

  runApp(const MyApp());
}

class BlocObs extends BlocObserver {
  @override
  void onEvent(Bloc bloc, Object? event) {
    super.onEvent(bloc, event);
    print(bloc.state);
  }

  @override
  void onTransition(Bloc bloc, Transition transition) {
    super.onTransition(bloc, transition);
    print(transition);
  }

  @override
  void onError(BlocBase bloc, Object error, StackTrace stackTrace) {
    super.onError(bloc, error, stackTrace);
    print("$error, $stackTrace");
  }

  @override
  void onChange(BlocBase bloc, Change change) {
    super.onChange(bloc, change);
    print("${change.currentState}, ${change.nextState}");
  }
}
```

---