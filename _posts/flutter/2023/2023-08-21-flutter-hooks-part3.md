---
title: Flutter - Hooks - useEffect
date: 2023-08-21 14:56:47 +/-09:00
category: [flutter/state]
tag: [flutter, state, hooks]
---

useEffect()는 initState() 및 didUpdateWidget, 그리고 dispose()와 같은 효과를 낼 수 있다.
아래 코드에서는 useEffect 내부에 컨트롤러의 변경값이 있을 때마다 useValue에 넣는 리스너를 추가했다.
그리고 그 변경값은 [] 안에 넣어준다.
마지막으로 return은 dispose와 같은 효과를 낼 수 있으며, 필요가 없을 시 null을 넣어주면 된다.
이렇듯 라이프사이클에 대한 전반적인 이해가 잘 갖춰져야 사용할 수 있다.

```dart
class MyApp extends HookWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    final useMyController = useTextEditingController();
    final useValue = useState("");
    useEffect(() {
      useMyController.addListener(() {
        useValue.value = useMyController.text;
      });
      return useMyController.dispose;
    },
    [useMyController]);
    
    
    return MaterialApp(
      home: Scaffold(
        body: Column(
          children: [
            TextField(
              controller: useMyController,
            ),
            Text(useValue.value),
          ],
        )
      )
    );
  }
}
```

요약:
1. 변경되는 값을 List에 넣고,
2. 함수 내의 실행 공간에서는 실행 명령을 넣는다.
3. return으로 dispose를 처리한다.

---