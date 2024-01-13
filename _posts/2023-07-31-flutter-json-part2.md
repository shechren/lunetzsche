---
title: Flutter - Json(2)
date: 2023-07-31 10:19:47 +/-09:00
category: [flutter/basic]
tag: [flutter, json]
---

[Do it! 깡샘의 플러터 & 다트 프로그래밍](https://www.yes24.com/Product/Goods/117206541)

본 코드는 위 책의 내용을 상당히 많이 참고했다.

```dart
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  State createState() {
    return MyAppState();
  }
}

class MyAppState extends State {
  String result = ''; // http req, res를 받는 string

  // http request의 get을 호출
  onPressGet() async {
    try {
      Map<String, String> headers = {
        'content-type': 'application/json',
        'accept': 'application/json',
      };
      http.Response response = await http.get(
          Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
          headers: headers);
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공
      if (response.statusCode == 200) {
        setState(() {
          result = response.body;
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 post를 호출
  onPressPost() async {
    try {
      http.Response response = await http.post(
          Uri.parse('https://jsonplaceholder.typicode.com/posts'),
          body: {'title': 'hello', 'body': 'world', 'userId': '1'});
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        setState(() {
          result = response.body;
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 client를 호출
  onPressClient() async {
    var client = http.Client();
    try {
      http.Response response = await client
          .post(Uri.parse('https://jsonplaceholder.typicode.com/posts'), body: {
        'title': '나의 일기장',
        'body': '오늘은 도서관에 와서 공부 중입니다.',
        'userId': '1'
      });
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        response ==
            await client
                .get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
        setState(() {
          result = response.body;
        });
      } else {
        print('error');
      }
    } finally {
      client.close();
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Test"),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text("$result"),
              ElevatedButton(
                onPressed: onPressGet,
                child: Text("Get"),
              ),
              ElevatedButton(
                onPressed: onPressPost,
                child: Text("Post"),
              ),
              ElevatedButton(onPressed: onPressClient, child: Text("Client")),
            ],
          ),
        ),
      ),
    );
  }
}
```

이 내용에서는 get, post, client 요청을 보내고 결괏값을 수신한다.

get에서는 header 내용을 정의하고 보내면 라틴어 격언으로 보이는 구절이 온다. get은 읽을 때 사용한다.

post는 말 그대로 포스트다. client도 똑같다. 차이점이란 post는 내장 http 클라이언트를 이용한다. 반대로 client는 http.Client() 객체를 이용한다.

---

이하는 json 파일로 정렬하는 방식이다.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  State createState() {
    return MyAppState();
  }
}

class MyAppState extends State {
  String result = ''; // http req, res를 받는 string
  Map<String, dynamic> responseData = {};

  // http request의 get을 호출
  onPressGet() async {
    try {
      Map<String, String> headers = {
        'content-type': 'application/json',
        'accept': 'application/json',
      };
      http.Response response = await http.get(
          Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
          headers: headers);
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공
      if (response.statusCode == 200) {
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 post를 호출
  onPressPost() async {
    try {
      http.Response response = await http.post(
          Uri.parse('https://jsonplaceholder.typicode.com/posts'),
          body: {'title': 'hello', 'body': 'world', 'userId': '1'});
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 client를 호출
  onPressClient() async {
    var client = http.Client();
    try {
      http.Response response = await client
          .post(Uri.parse('https://jsonplaceholder.typicode.com/posts'), body: {
        'title': '나의 일기장',
        'body': '오늘은 도서관에 와서 공부 중입니다.',
        'userId': '1'
      });
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        response ==
            await client
                .get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
        });
      } else {
        print('error');
      }
    } finally {
      client.close();
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Test"),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              ElevatedButton(
                onPressed: onPressGet,
                child: Text("Get"),
              ),
              ElevatedButton(
                onPressed: onPressPost,
                child: Text("Post"),
              ),
              ElevatedButton(onPressed: onPressClient,
                  child: Text("Client")
              ),
              if (responseData.isNotEmpty)
                Table(
                children: [
                  TableRow(
                    children: [
                      Text(responseData["title"].toString()),
                      Text(responseData["body"].toString()),
                      Text(responseData["userID"].toString()),
                    ],
                  )
                ],
              ) else Text("No data")
            ],
          ),
        ),
      ),
    );
  }
}
```

이렇게 테이블로 정렬할 수도 있다.

responseData라는 Map 객체를 만들고, result를 json 형식으로 변환한다. 그리고 Text로 다시 변환한다.

---

만약에 List로 변환한다면 리스트의 개수에 따라 유동적인 반환이 가능해진다.

```dart
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

void main() {
  runApp(MyApp());
}

class MyApp extends StatefulWidget {
  @override
  State createState() {
    return MyAppState();
  }
}

class MyAppState extends State {
  String result = ''; // http req, res를 받는 string
  Map<String, dynamic> responseData = {};
  List responseList = [];

  // http request의 get을 호출
  onPressGet() async {
    try {
      Map<String, String> headers = {
        'content-type': 'application/json',
        'accept': 'application/json',
      };
      http.Response response = await http.get(
          Uri.parse('https://jsonplaceholder.typicode.com/posts/1'),
          headers: headers);
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공
      if (response.statusCode == 200) {
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
          responseList =
              responseData.entries.map((e) => '${e.key}: ${e.value}').toList();
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 post를 호출
  onPressPost() async {
    try {
      http.Response response = await http.post(
          Uri.parse('https://jsonplaceholder.typicode.com/posts'),
          body: {'title': 'hello', 'body': 'world', 'userId': '1'});
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
          responseList =
              responseData.entries.map((e) => '${e.key}: ${e.value}').toList();
        });
      } else {
        print("error");
      }
    } catch (e) {
      print("error $e");
    }
  }

  // http request의 client를 호출
  onPressClient() async {
    var client = http.Client();
    try {
      http.Response response = await client
          .post(Uri.parse('https://jsonplaceholder.typicode.com/posts'), body: {
        'title': '나의 일기장',
        'body': '오늘은 도서관에 와서 공부 중입니다.',
        'userId': '1'
      });
      print('statusCode : ${response.statusCode}');
      // http 상태코드가 200 = 성공, 201 = 성공 뒤 작성
      if (response.statusCode == 200 || response.statusCode == 201) {
        response ==
            await client
                .get(Uri.parse('https://jsonplaceholder.typicode.com/posts'));
        setState(() {
          result = response.body;
          responseData = jsonDecode(result);
          responseList =
              responseData.entries.map((e) => '${e.key}: ${e.value}').toList();
        });
      } else {
        print('error');
      }
    } finally {
      client.close();
    }
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Test"),
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              ElevatedButton(
                onPressed: onPressGet,
                child: Text("Get"),
              ),
              ElevatedButton(
                onPressed: onPressPost,
                child: Text("Post"),
              ),
              ElevatedButton(onPressed: onPressClient, child: Text("Client")),
              if (responseData.isNotEmpty)
                Table(
                  children: [
                    TableRow(
                      children: [
                        for (var i in responseList)
                          Text(i, style: TextStyle(
                            fontSize: 20,
                          )),
                      ],
                    )
                  ],
                )
              else
                Text("No data")
            ],
          ),
        ),
      ),
    );
  }
}
```

---