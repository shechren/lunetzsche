---
title: Flutter - 테마 컴포넌트화
date: 2023-07-09 23:12:13 +/-09:00
category: [flutter/basic]
tag: [flutter, component]
---

[https://github.com/shechren/DIM/tree/master/Do3](https://github.com/shechren/DIM/tree/master/Do3)

코드는 위 링크에 있다.
```dart
class Home extends StatelessWidget {
  const Home({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
            child: Column(
              children: [
                const SizedBox(height: 20, width: 20),
                Row(
                  children: [
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.primary,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.secondary,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.tertiary,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                  ],
                ),
                const SizedBox(height: 20, width: 20),
                Row(
                  children: [
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.primaryContainer,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.secondaryContainer,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                    const SizedBox(width: 10, height: 10),
                    Container (
                      width: 100,
                      height: 100,
                      color: Theme.of(context).colorScheme.tertiaryContainer,
                      child: const Text("동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                        overflow: TextOverflow.ellipsis,
                        maxLines: 4,
                        style: TextStyle(fontSize: 15),
                      ),
                    ),
                  ],
                )
              ],
            )
        )
    );
  }
}
```
컨테이너는 계속해서 반복되고 있다. Row가 반복될 때마다 color값만 바꿔서 말이다.

우리는 color값만이 아니라 text도 그때그때 바꿀 것이다.

즉, width와 height 등의 값을 매번 입력하지 않고 컴포넌트에서 처리하도록 할 것이다.
```dart
class ConWidget extends StatelessWidget {
  final Color color;
  final String text;
  const ConWidget({
    Key? key,
    required this.color,
    required this.text,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 100,
      height: 100,
      decoration: BoxDecoration(
        color: color,
      ),
      child: Text(
        text,
        overflow: TextOverflow.ellipsis,
        maxLines: 4,
        style: const TextStyle(fontSize: 15),
      ),
    );
  }
}
```
컴포넌트화를 위해 컨테이너를 클래스로 만들 것이다.

color와 text는 final로 바꿔서 컴파일 시점이 아닌 런타임 시점에 값을 대입할 것이다.

생성자 ConWidget에서 Key를 required가 아닌 null을 허용하는 값으로 바꾼다. 그리고 color와 key는 required로 바꾼다.

color 값을 decoration으로 바꾼다.
```dart
ConWidget(
  key: UniqueKey(),
  color: Theme.of(context).colorScheme.primary,
  text: "동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
),
```
이런 식으로 색깔과 내용을 계속해서 바꿔나가면 된다.
```dart
class Home extends StatelessWidget {
  const Home({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
            child: Column(
              children: [
                const SizedBox(height: 20, width: 20),
                Row(
                  children: [
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.primary,
                      text: "동해물과 백두산이 마르고 닳도록 하느님이 보우하사 우리나라 만세",
                    ),
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.secondary,
                      text:
                      "울산성 전투는 임진왜란에서 가장 큰 전투이며 조명 연합군 58,000여명이 동원되었으나 공격이 좌절되었다.",
                    ),
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.tertiary,
                      text:
                      "플로렌스 포스터 젠킨스는 음치로 유명하지만 카네기 홀에서 밤의 여왕의 아리아를 선보인 적이 있다.",
                    ),
                  ],
                ),
                const SizedBox(height: 20, width: 20),
                Row(
                  children: [
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.primaryContainer,
                      text:
                      "톰 크린은 로버트 스콧 그리고 어니스트 섀클턴 모두와 함께 여행을 떠나본 기록이 있으며 3번의 남극 탐험에서 모두 생환했다.",
                    ),
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.secondaryContainer,
                      text:
                      "케르마데크 해구는 세계에서 2번째로 깊은 해구이지만 마리아나 해구에 비해 상대적으로 덜 알려진 지명이다.",
                    ),
                    const SizedBox(width: 10, height: 10),
                    ConWidget(
                      key: UniqueKey(),
                      color: Theme.of(context).colorScheme.tertiaryContainer,
                      text:
                      "프로그래머가 되려면 정말로 여러가지 노력을 필요로 한다. 수학, 영어는 기본이며 컴퓨터식 사고를 필수로 한다.",
                    ),
                  ],
                )
              ],
            )
        )
    );
  }
}
```
아까보다 가독성이 훨씬 나아졌다. 물론 고작 6개를 바꾼 것이긴 하지만, 이것이 수십개가 되면 컴포넌트화가 얼마나 크게 의미있는지 상상이 갈 것이다.

---