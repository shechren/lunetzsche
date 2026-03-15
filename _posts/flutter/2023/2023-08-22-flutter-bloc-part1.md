---
title: Flutter - bloc (1)
date: 2023-08-22 12:01:35 +/-09:00
category: [flutter/state]
tag: [flutter, state, bloc]
---

```terminal
flutter pub add bloc flutter_bloc
flutter pub get
```


Bloc의 개념을 잡고 넘어가자.

우리의 프론트엔드와 백엔드는 서로 밀접하게 연계되어 있다. 프론트엔드에서 백엔드에 요청하면 백엔드는 그 결괏값을 도출하여 프론트엔드에 반환한다. 이것이 기본 네트워크의 개념인 것은 초등학생도 알 것이다.
그러나 사용자는 편할지 몰라도 유지 및 보수에서는 치명적이다. 프론트에서 하나만 수정해도 백엔드에 곧장 영향이 가며, 그 반대의 경우도 마찬가지다.
그렇기에 Bloc은 그 연결 자체를 중간에서 가로채버린다. 프론트엔드에서 이벤트를 보내면 Bloc이 가로채고, 백엔드에서 결과를 보내도 Bloc이 가로챈다. 이것이 Bloc의 기본 개념이다. 우리는 이벤트 개념을 Bloc에 보냄으로서 Bloc에 시킬 일이 무엇인지 알려주고, 프론트엔드와 백엔드는 각자 자유롭게 자신의 틀을 수정할 수 있게 되는 것이다.

Bloc에서 이벤트를 선언하는 법은 크게 다음과 같다.

* enum

```dart
enum MyEvent {myFirstEvent, mySecondEvent }
```

열거형 상수로 선언하는 방법이 있다. 이후 이것을 꺼내 쓰고 싶다면 MyEvent.myFirstEvent 또는 MyEvent.mySecondEvent 이런 식으로 꺼내서 쓰면 된다.

* class

```dart
abstract class MyEventClass {}

class MyFirstEventClass extends MyEventClass {}
class MySecondEventClass extends MyEventClass {}
```

또 하나는 추상 클래스로 선언하는 방법이다. 추상 클래스는 기능들의 네이밍만 선언을 하고 자식 클래스에 그대로 상속한다. 이렇게 하면 자식 클래스는 반드시 해당 기능들을 써야 한다. 물론 그냥은 넘길 수 없고, 생성자에 this.프로퍼티를 넣어주면 된다.

```dart
abstract class MyEventClass {
  String name;
  MyEventClass(this.name);
  
}

class MyFirstEventClass extends MyEventClass {
  MyFirstEventClass(name) : super(name);

}

class MySecondEventClass extends MyEventClass {
  MySecondEventClass(name) : super(name);
}
```

[bloc 공식 레퍼런스](https://pub.dev/packages/flutter_bloc)

bloc은 공식 레퍼런스가 굉장히 직관적이고 상세하게 존재한다.

[flutter_bloc package description](https://bloclibrary.dev/#/flutterbloccoreconcepts)

위 링크에서도 상세한 설명을 볼 수 있다.

---