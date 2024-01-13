---
title: Flutter - Json(1)
date: 2023-07-30 09:23:24 +/-09:00
category: [flutter/basic]
tag: [flutter, json]
---

우선 JSON 파일을 json/data.json에 만든다.
```json
{
 "LapisTerritory": {
 "Escha": {
 "name": "Escha Lapis Schnabel",
 "age": 18,
 "weapon": "Rapier"
  },
 "Inar": {
 "name": "Inar Lopend Nordfeldt",
 "age": 20,
 "weapon": "Estoc"
  },
 "Snow": {
 "name": "Snow Nierrabit Lawrence",
 "age": 3,
 "weapon": "Estoc"
  },
 "Spade" : {
 "name" : "Spade Rosen",
 "age" : 22,
 "weapon" : "Spata"
    }
  }
}
```

이것을 이제 사람별로 하나씩 출력할 차례다.
```dart
import 'dart:convert';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatefulWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  State createState() => _MyAppState();
}

class _MyAppState extends State {
  Map<String, dynamic>? data;

  @override
  void initState() {
    super.initState();
    getData();
  }

  Future getData() async {
    final String jsonString = await rootBundle.loadString('json/data.json');
    setState(() {
      Map<String, dynamic> data = jsonDecode(jsonString);
      Map<String, dynamic> characters = data['LapisTerritory'];
      characters.forEach((key, value) {
        print('${value['name']}, ${value['age']}, ${value['weapon']}');
      });
    });
  }

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Text("테스트"),
      ),
    );
  }
}
```

1\. dart:convert와 flutter/services.dart를 import한다.

2\. Future를 연 뒤 async 비동기 함수를 시작한다. jsonString은 json 파일을 받아온다.

setState 내에서는 String, dynamic으로 매핑된 Map을 가져온다. data를 1차로 받아오고, characters로 2차로 솎아낸다.

마지막으로 forEach문을 key와 value로 돌려서 Map을 분해한다.

![flutter-json.png](/assets/postingImage/flutter-json.png)

---