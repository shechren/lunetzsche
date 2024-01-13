---
title: Flutter - Hooks - ValueNotifier
date: 2023-08-18 16:49:28 +/-09:00
category: [flutter/state]
tag: [flutter, state, hooks]
---

useState() 부분을 보면 인자로 ValueNotifier를 받는다는 것을 알 수 있다.
즉, 우리는 ValueNotifier를 통해 build 함수 밖에서 useState()를 이용할 수 있게 된다.

```dart
final ValueNotifier<String?> name;
final ValueNotifier<int?> age;
```

물론 해당 클래스 내에서만 빌드 함수가 안 쓰인다는 거지, final로 상속할 적에는 당연히 useState()의 빌드 함수를 넘겨줬다.
아무튼 이런 방식을 통해 useState()에 대해 상속을 쉽게 할 수 있다.

---