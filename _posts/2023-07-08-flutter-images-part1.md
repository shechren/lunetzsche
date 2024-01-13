---
title: Flutter - Images(1)
date: 2023-07-08 13:19:20 +/-09:00
category: [flutter/basic]
tag: [flutter, image]
---

[Do it! 깡샘의 플러터 & 다트 프로그래밍](https://www.yes24.com/Product/Goods/117206541)

본 내용은 위 책을 많이 참고했다.

이미지 파일은 다음과 같이 등록할 수 있다.

1. 필요한 이미지를 경로에 넣는다.

2. pubspec.yaml 파일을 다음과 같이 수정한다.
```yaml
flutter:
  uses-material-design: true
# import image files.
# assets : - images/name.png
  assets:
    - images/
```
images/는 해당 폴더 내의 전체 요소를 불러오겠다는 뜻이다.(하위 디렉토리 로드 불가능. 하위 디렉토리는 별도로 지정해줘야 함.)

3. 이미지 위젯을 사용한다.
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar: AppBar(
            title: Text("images Loading"),
          ),
          body: Row(textDirection: TextDirection.ltr, children: [
            Image.asset('images/1.png'),
            Image.asset('images/2.png'),
            Image.asset('images/3.png'),
          ])),
    );
  }
}
```
Row의 경우 textDirection까지 등록해줘야 한다. Column의 경우 그냥 한다.

![flutter-images-1.png](/assets/postingImage/flutter-images-1.png)

화면 크기에 맞지 않을 경우 이미지가 잘리는 것을 볼 수 있다.

이미지 크기에 따라 다른 에셋을 적용할 수도 있다. 파일 이름은 같지만 하위 디렉토리가 다를 경우 플루터가 알아서 이해하여 이미지를 구분해준다. 즉, images/image.png와 images/second/image.png를 플루터에서 구분해서 처리해준다는 뜻이다. 다만 이미지 크기로 구분할 경우 2.0x, 3.0x 같이 폴더 이름을 지어야 한다.

안드로이드: hdpi, xhdpi, xxhdpi, xxxhdpi ...

아이폰: 1x, 2x, 3x ...
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar: AppBar(
            title: Text("images Loading"),
          ),
          body: Row(textDirection: TextDirection.ltr, children: [
            Image(image: AssetImage('images/1.png')),
            Image(image: AssetImage('images/2.png')),
            Image(image: AssetImage('images/3.png')),
          ])),
    );
  }
}
```
이미지는 위와 같이도 올릴 수 있다.

이미지의 크기는 다음과 같이 조절한다.
```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
          appBar: AppBar(
            title: Text("images Loading"),
          ),
          body: Row(textDirection: TextDirection.ltr, children: [
            Image(image: ResizeImage(AssetImage('images/1.png'), width: 250, height: 250)),
            Image(image: ResizeImage(AssetImage('images/2.png'), width: 250, height: 250)),
            Image(image: ResizeImage(AssetImage('images/3.png'), width: 250, height: 250)),
          ]
        )
      ),
    );
  }
}
```
![flutter-images-2.png](/assets/postingImage/flutter-images-2.png)

---