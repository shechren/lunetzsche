---
title: Flutter - 파이어스토어(Firestore) 글 저장하기 2
date: 2023-08-19 11:42:06 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase, firestore]
---

지금까지 우리는 파이어스토어에 글을 올렸는데, 문제는 name으로 이미 doc이 네이밍되어 있어 계속해서 값이 바뀐다는 점이다. 따라서 우리는 새로운 값을 저장할 필요성이 있다.

이번에는 main 함수 내의 부분을 MyGet.dart로 옮기고, MyGet은 이름을 바꿔 MyRequest로 해보자.

MyRequest.dart
```dart
class MyWrite extends HookWidget {
  MyWrite({super.key});

  @override
  Widget build(BuildContext context) {
    final useFormKey = useMemoized(() => GlobalKey<FormState>());
    var useName = useState(MyInherited.of(context)!.name);
    var useAge = useState(MyInherited.of(context)!.age);

    myFireStoreAdd() async {
      useFormKey.currentState!.save();
      final db = FirebaseFirestore.instance;
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

    return Form(
        key: useFormKey,
        child: MyInherited(
          child: MyForm(
            name: useName,
            age: useAge,
            FireStoreFunction: myFireStoreAdd,
          ),
        ));
  }
}
```
메인함수에 남은 부분은 이제 스스로도 정리할 수 있을 것이기에 여기에 적지 않는다.

---

사실 간단한데, update가 아닌 새로운 write 효과를 얻기 위해서는 await 부분에서 받는 인자를 수정해주면 된다.

```dart
myFireStoreAdd() async {
  useFormKey.currentState!.save();
  final db = FirebaseFirestore.instance;
  CollectionReference myRef = db.collection("users");
  Map<String, dynamic> myMap = {
    'name': useName.value,
    'age': useAge.value,
  };
  try {
    await myRef.doc().set(myMap);
    print("${useName.value}, ${useAge.value} 저장 완료");
  } catch (e) {
    print("연결이 실패했습니다: $e");
  }
}
```
myRef.doc("name") 을 myRef.doc()로 수정해준다.
즉, 반대로 말하면, update 효과를 얻기 위해서는 name에 where가 들어가면 된다는 것이다.

---

이제 값이 update 될 때마다 화면을 갱신해보자. MyGet함수를 수정하면 되는데 이건 조금 어렵다.

```dart
class MyGet extends HookWidget {
  const MyGet({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final myDataList = useState<List<MyData>>([]);

    myFireStoreGet() {
      final db = FirebaseFirestore.instance
          .collection('users')
          .snapshots()
          .listen((QuerySnapshot querySnapshot) {
        final newDataList = querySnapshot.docs
            .map((doc) => MyData.fromJson(doc.data() as Map<String, dynamic>))
            .toList();
        myDataList.value = newDataList;
      });

      return () {
        db.cancel();
      };
    }

    useEffect(() {
      myFireStoreGet();
      return null;
    }, []);

    return myDataList.value.isEmpty
        ? const Center(child: CircularProgressIndicator())
        : ListView.builder(
            itemCount: myDataList.value.length,
            itemBuilder: (BuildContext context, int index) {
              return ListTile(
                title: Text(myDataList.value[index].name),
                subtitle: Text(myDataList.value[index].age.toString()),
              );
            },
          );
  }
}
```

제일 먼저 myDataList는 동적이어야 하니 useState()로 바꿔준다.
.collection()으로 컬렉션에 접근한다.
.snapshot으로 Stream을 가져온다. **그래서 async와 await을 사용하지 않는 것이다.**
.listen으로 snapshot을 리스닝한다.
이제 이 안에 새로운 함수가 들어간다.
newDataList에 받아온 정보를 넣고,
myDataList.value에 대입한다.

마지막으로 db.cancel()을 통해 리스너를 강제 취소한다.

---