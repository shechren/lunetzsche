---
title: Flutter - 파이어스토어(Firestore) 내용 저장하기 1
date: 2023-08-19 10:43:53 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase, firestore]
---

지난번 코드를 이어서 쓸 거지만, 너무 난잡하니 일단 상태 관리 Inherited Widget을 통해 조금 다듬기로 한다.

main.dart
```dart
import 'package:firebase_core/firebase_core.dart';
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:temp/Inherited.dart';
import 'MyForm.dart';
import 'firebase_options.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(MyInherited(child: MyApp()));
}

class MyApp extends HookWidget {
  MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    final useFormKey = useMemoized(() => GlobalKey<FormState>());
    var useName = useState(MyInherited.of(context)!.name);
    var useAge = useState(MyInherited.of(context)!.age);

    myFireStoreAdd() async {
      useFormKey.currentState!.save();
      FirebaseFirestore db = FirebaseFirestore.instance;
      CollectionReference myRef = db.collection("users");
      Map<String, dynamic> myMap = {
        'name': useName.value,
        'age': useAge.value,
      };
      try {
        await myRef.doc('name').set(myMap);
        print("${useName.value}, ${useAge.value} 저장 완료");
      } catch (e) {
        print("연결이 실패했습니다: $e");
      }
    }

    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
            ),
            body: Center(
              child: Form(
                key: useFormKey,
                child: MyInherited(child: MyForm(name: useName, age: useAge, FireStoreFunction: myFireStoreAdd,),)
              ),
            )));
  }
}
```

MyForm.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_hooks/flutter_hooks.dart';

class MyForm extends HookWidget {
  const MyForm(
      {required this.name,
      required this.age,
      required this.FireStoreFunction,
      super.key});
  final FireStoreFunction;
  final name;
  final age;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        TextFormField(onSaved: (String? value) {
          if (value != null && value.isNotEmpty) {
            name.value = value;
          }
        }),
        const SizedBox(height: 10),
        TextFormField(
            keyboardType: TextInputType.number,
            onSaved: (String? value) {
              if (value != null && value.isNotEmpty) {
                age.value = int.parse(value);
              }
            }),
        ElevatedButton(onPressed: FireStoreFunction, child: const Text("저장")),
      ],
    );
  }
}
```

Inherited.dart
```dart
import 'package:flutter/material.dart';

class MyInherited extends InheritedWidget {
  String? name;
  int? age;

  MyInherited({
    super.key,
    required Widget child,
  }) : super(child: child);

  @override
  bool updateShouldNotify(covariant InheritedWidget oldWidget) => true;

  static MyInherited? of(BuildContext context) =>
      context.dependOnInheritedWidgetOfExactType();
}
```

---

하지만 이제부터다. 파일은 점점 늘어날 것이다. 우리는 class로 json 파일의 형식을 정의할 것이다.

MyData.dart
```dart
class MyData {
  MyData({required this.name, required this.age});

  String name;
  int age;

  MyData.fromJson(Map<String, dynamic> json) :
      name = json['name'],
  age = json['age'];

  Map<String, dynamic> toJson() => {
    'name' : name,
    'age' : age,
  };
}
```
toJson은 json 형식으로 만드는 것, fromJson은 json 형식을 가져와 프로퍼티에 대입하는 것이다.

이제 파이어스토어에서 데이터를 가져오는 파일을 만든다.

MyGet.dart
```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter_hooks/flutter_hooks.dart';
import 'package:flutter/material.dart';

import 'MyData.dart';

class MyGet extends HookWidget {
  MyGet({super.key});

  List<MyData> myDataList = [];

  Future<List<MyData>> myFireStoreGet() async {
    FirebaseFirestore db = FirebaseFirestore.instance;
    CollectionReference myColRef = db.collection("users");
    QuerySnapshot querySnapshot = await myColRef.get();
    List<QueryDocumentSnapshot> myList = querySnapshot.docs;
    List<MyData> myDataList = myList.map((doc) =>
        MyData.fromJson(doc.data() as Map<String, dynamic>)).toList();
    return myDataList;
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder<List<MyData>>(
      future: myFireStoreGet(),
      builder: (BuildContext context, AsyncSnapshot<List<MyData>> snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return const Center(child: CircularProgressIndicator());
        }
        if (!snapshot.hasData || snapshot.data!.isEmpty) {
          return const Text("데이터가 없습니다.");
        }
        List<MyData> myDataList = snapshot.data!;
        return ListView.builder(
          itemCount: myDataList.length,
          itemBuilder: (BuildContext context, int index) {
            return ListTile(
              title: Text(myDataList[index].name),
              subtitle: Text(myDataList[index].age.toString()),
            );
          },
        );
      },
    );
  }
}
```

콜백을 await로 수신하지 않으면 무조건 "데이터가 없습니다." 만 출력된다. 서버의 시간과 내 프로세스의 시간이 다르기 때문이다.
await를 썼기에 당연히 return값을 줘야 하고, List를 반환해준다.
빌드 함수에서는 퓨처 빌더를 사용하나 async를 사용하지 않는다. 조건문을 적어둔 뒤, ListView.builder를 사용해 값을 정렬해준다.

마지막으로 main.dart의 MyApp 함수의 return을 수정해준다.

```dart
return MaterialApp(
      home: Scaffold(
          appBar: AppBar(),
          body: Column(children: [
            Form(
                key: useFormKey,
                child: MyInherited(
                  child: MyForm(
                    name: useName.value,
                    age: useAge.value,
                    FireStoreFunction: myFireStoreAdd,
                  ),
                )),
            Expanded(child: MyGet()),
          ])));
```

이렇게 하면 내가 추가한 데이터를 열람할 수 있다.

---