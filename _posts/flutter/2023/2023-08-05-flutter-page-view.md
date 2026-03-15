---
title: Flutter - PageView
date: 2023-08-05 17:58:31 +/-09:00
category: [flutter/basic]
tag: [flutter, PageView, image]
---

일단 이미지 5개가 필요하다. 각각 1부터 5까지 이미지 이름을 정하고, assets/images 폴더에 넣는다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart' show rootBundle;
import 'dart:convert';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final manifestContent = await rootBundle.loadString('AssetManifest.json');
  final Map<String, dynamic> manifestMap = json.decode(manifestContent);

  final imagePaths = manifestMap.keys
      .where((key) => key.contains('assets/images/'))
      .toList();

  runApp(_Screen(imagePaths: imagePaths));
}

class _Screen extends StatefulWidget {
  final List<String> imagePaths;

  const _Screen({Key? key, required this.imagePaths}) : super(key: key);

  @override
  State<_Screen> createState() => _ScreenState();
}

class _ScreenState extends State<_Screen> {
  late final PageController _pageController;

  @override
  void initState() {
    super.initState();
    _pageController = PageController(initialPage: 0);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: GestureDetector(
          onTap: () {
            _pageController.animateToPage(
                (_pageController.page!.toInt() + 1) % widget.imagePaths.length,
                duration: Duration(milliseconds: 500),
            curve: Curves.easeInOut,
            );
          },
          child: PageView(
            controller: _pageController,
            children: [
              for (int i = 0; i < widget.imagePaths.length; i++)
                Image.asset(widget.imagePaths[i], fit: BoxFit.cover)
            ],
          ),
        ),
      ),
    );
  }
}
```

코드를 분해할 시간이다.

**메인 함수**

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final manifestContent = await rootBundle.loadString('AssetManifest.json');
  final Map<String, dynamic> manifestMap = json.decode(manifestContent);

  final imagePaths = manifestMap.keys
      .where((key) => key.contains('assets/images/'))
      .toList();

  runApp(_Screen(imagePaths: imagePaths));
}

  ```

* **WidgetsFlutterBinding.ensureInitialized**는 WidgetsBinding을 초기화해준다.
* 파일을 불러오기 위해 async를 사용한다.
* 'AssetManifest.json' 파일은 assets 폴더 내의 정보를 가져올 수 있다. 즉, 파일 이름과 수를 json으로 가져온다.
* **manifestMap**을 분해해보면 다음과 같은 결과가 추출된다.
  * {assets/images/1.png: [assets/images/1.png], assets/images/2.png: [assets/images/2.png], assets/images/3.png: [assets/images/3.png], assets/images/4.jpg: [assets/images/4.jpg], assets/images/5.jpg: [assets/images/5.jpg], packages/cupertino_icons/assets/CupertinoIcons.ttf: [packages/cupertino_icons/assets/CupertinoIcons.ttf]}
  
  이 Map 정보를 파일 이름을 key로 하여 정렬하고, 리스트로 치환한다.

**Screen 함수**
```dart
class _Screen extends StatefulWidget {
  final List<String> imagePaths;

  const _Screen({Key? key, required this.imagePaths}) : super(key: key);

  @override
  State<_Screen> createState() => _ScreenState();
}

class _ScreenState extends State<_Screen> {
  late final PageController _pageController;

  @override
  void initState() {
    super.initState();
    _pageController = PageController(initialPage: 0);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: GestureDetector(
          onTap: () {
            _pageController.animateToPage(
                (_pageController.page!.toInt() + 1) % widget.imagePaths.length,
                duration: Duration(milliseconds: 500),
            curve: Curves.easeInOut,
            );
          },
          child: PageView(
            controller: _pageController,
            children: [
              for (int i = 0; i < widget.imagePaths.length; i++)
                Image.asset(widget.imagePaths[i], fit: BoxFit.cover)
            ],
          ),
        ),
      ),
    );
  }
}
```

* 페이지를 컨트롤하기 위한 페이지 컨트롤러를 생성한다.
* required imagePaths를 넣어 이미지 경로 값을 반드시 전달받는다.
* Page를 생성하고, GestureDetector를 통해 사진을 누르면 다음 화면으로 넘어가지게 만든다.
* 페이지 컨트롤러는 0.5초의 시간동안 자연스럽게 화면을 넘기는 효과를 추가한다.
* PageView에서 컨트롤러를 등록하고, for문으로 이미지 수에 맞도록 에셋을 불러온다.

```dart
  @override
  void dispose() {
    _pageController.dispose();
    super.dispose();
  }
```

마지막으로 메모리의 안전한 정리를 위해 컨트롤러도 dispose를 해준다.

[전체 코드](https://github.com/shechren/DIM/blob/master/do11/lib/main.dart)

---