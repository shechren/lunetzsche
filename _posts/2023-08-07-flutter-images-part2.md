---
title: Flutter - Images(2)
date: 2023-08-07 15:34:28 +/-09:00
category: [flutter/basic]
tag: [flutter, image, imagepicker]
---

이번 시간에는 imagePicker를 이용해볼 것이다. asset이 아닌 폴더에 적용된 것도 불러올 것이다.

[전체 코드](https://github.com/shechren/DIM/blob/master/do13/lib/main.dart)

먼저 image_picker를 인스톨한다.

```terminal
flutter pub add image_picker
flutter pub get
```

그다음 우리의 메인 화면을 만든다.

```dart
Widget notImage () {
    return GestureDetector(
      onTap: imageAdd,
      child: const Icon(Icons.add, size: 100,),
    );
  }
```

아이콘을 누르면 imageAdd 함수가 작동되게 한다.

```dart
class _HomeState extends State<HomeState> {
  XFile? receiveImage;

  void imageAdd() async {
    final myImage = await ImagePicker().pickImage(source: ImageSource.gallery);

    setState(() {
      receiveImage = myImage;
    });
  }
```

async를 통해 await을 열고 **ImagePicker().pickImage()**를 받는다.
상태가 바뀌면 setState를 통해 receiveImage 함수에 넣는다. receiveImage 함수는 XFile 형식이다.

```dart
  Widget renderImage () {
    return Image.network(receiveImage!.path);
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: receiveImage == null ? notImage() : renderImage(),
      )
    );
  }
}
```
이미지를 불러온다. receiveImage가 널일 경우 이미지 추가 아이콘을 띄우고, 널이 아닐 경우 renderImage를 띄운다.
renderImage는 Image.network()로 받아온다.

---