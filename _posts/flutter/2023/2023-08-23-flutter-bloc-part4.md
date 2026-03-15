---
title: Flutter - bloc (4) - Bloc Widgets
date: 2023-08-23 19:11:43 +/-09:00
category: [flutter/state]
tag: [flutter, state, bloc]
---

여기서는 **Bloc Widgets**에 대해서 다룬다. 상세한 내용은 [이 링크](https://bloclibrary.dev/#/flutterbloccoreconcepts?id=bloc-widgets)를 참조하기 바란다.

- [**1. BlocBuilder**](#blocbuilder)
- [**2. BlocSelector**](#blocselector)
- [**3. BlocProvider**](#blocprovider)
- [**4. MultiBlocProvider**](#multiblocprovider)
- [**5. BlocListener**](#bloclistener)
- [**6. MultiBlocListener**](#multibloclistener)
- [**7. BlocConsumer**](#blocconsumer)
- [**8. RepositoryProvider**](#repositoryprovider)
- [**9. MultiRepositoryProvider**](#multirepositoryprovider)

# **BlocBuilder**

블록의 기본적인 빌더 함수다.

```dart
BlocBuilder<BlocA, BlocAState>(
  bloc: blocA,
  buildWhen: (previousState, state) {
    // state의 값이 true냐 false에 따라 return한다.
    // true이면 builder가 호출되면 state가 다시 빌드된다. false이면 state가 빌드되지 않는다.
  }
  builder: (context, state) {
    // BlocA의 상태 위젯을 리턴한다.
  }
)
```

# **BlocSelector**

블록 빌더와 유사하지만 state를 반환한다.

```dart
BlocSelector<BlocA, BlocAState, SelectedState>(
  selector: (state) {
    // 현재 Bloc state를 기반으로 app state를 반환하는 함수
  },
  builder: (context, state) {
    // 선택된 app state를 사용하여 위젯을 빌드하는 함수

  },
)
```

# **BlocProvider**

Provider라는 이름이 붙은 것처럼 **의존성**을 부여한다.
크게 2가지의 패턴이 있다.

```dart
BlocProvider(
  lazy: false,
  create: (BuildContext context) => BlocA(),
  child: ChildA(),
);
```
lazy는 지연 초기화를 의미한다. 이 bloc 인스턴스에 접근할 때까지 초기화하지 않으며 앱의 성능을 향상시킨다.

```dart
BlocProvider.value(
  value: BlocProvider.of<BlocA>(context),
  child: ScreenA(),
);
```
BlocProvider와 BlocProvider.value의 차이는 create의 유무다. value의 경우 이미 생성된 value를 전달하기에 라이프사이클을 관리하지 않아도 되지만 BlocProvider는 동적으로 인스턴스를 생성한다.
그리고 BlocProvider는 상위에서 사용되지만 value는 하위 위젯과 동일한 범위에서 인스턴스를 공유하기 위해 사용된다.

# **MultiBlocProvider**

다수의 블록을 묶어준다.

```dart
MultiBlocProvider(
  providers: [
    BlocProvider<BlocA>(
      create: (BuildContext context) => BlocA(),
    ),
    BlocProvider<BlocB>(
      create: (BuildContext context) => BlocB(),
    ),
    BlocProvider<BlocC>(
      create: (BuildContext context) => BlocC(),
    ),
  ],
  child: ChildA(),
)
```

# **BlocListener**

```dart
BlocListener<BlocA, BlocAState>(
  bloc: blocA,
  listener: (context, state) {
    // do stuff here based on BlocA's state
  },
  child: Container()
)
```

BlocListener는 상태가 변경될 때 bloc을 호출해준다. bloc을 지정하지 않으면 현재 BlocProvider의 context를 조회하여 자동으로 실행한다. context를 통해 접근할 수 없는 bloc이라면 지정해주면 된다.

```dart
BlocListener<BlocA, BlocAState>(
  listenWhen: (previousState, state) {
    // return true/false to determine whether or not
    // to call listener with state
  },
  listener: (context, state) {
    // do stuff here based on BlocA's state
  },
  child: Container(),
)
```
함수가 호출되는 시점을 listenWhen을 통해 조절할 수 있다. 과거 state와 현재 state를 가져와 bool로 반환한다. true일 경우 listener는 state를 호출하며 false일 경우 호출하지 않는다.
하위 위젯을 re-build하지 않는다. 즉, 상태 변화를 받기만 할 뿐이다.

# **MultiBlocListener**

```dart
MultiBlocListener(
  listeners: [
    BlocListener<BlocA, BlocAState>(
      listener: (context, state) {},
    ),
    BlocListener<BlocB, BlocBState>(
      listener: (context, state) {},
    ),
    BlocListener<BlocC, BlocCState>(
      listener: (context, state) {},
    ),
  ],
  child: ChildA(),
)
```

# **BlocConsumer**

```dart
BlocConsumer<BlocA, BlocAState>(
  listener: (context, state) {
    // do stuff here based on BlocA's state
  },
  builder: (context, state) {
    // return widget here based on BlocA's state
  }
)
```

BlocBuilder와 BlocListener를 합친 기능이다.

# **RepositoryProvider**

```dart
RepositoryProvider(
  create: (context) => RepositoryA(),
  child: ChildA(),
);
```

저장소 클래스다. 위젯이 저장소를 이용할 수 있게 만든다. 저장소라 함은 로컬 메모리 혹은 서버의 데이터베이스를 떠올리는데, 그것이 맞다. 따라서 우리는 여기에서 데이터베이스 프로그래밍을 할 수 있다.

# **MultiRepositoryProvider**

```dart
MultiRepositoryProvider(
  providers: [
    RepositoryProvider<RepositoryA>(
      create: (context) => RepositoryA(),
    ),
    RepositoryProvider<RepositoryB>(
      create: (context) => RepositoryB(),
    ),
    RepositoryProvider<RepositoryC>(
      create: (context) => RepositoryC(),
    ),
  ],
  child: ChildA(),
)
```

---