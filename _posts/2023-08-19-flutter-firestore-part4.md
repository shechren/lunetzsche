---
title: Flutter - 파이어스토어(Firestore) 내용 삭제하기
date: 2023-08-19 15:59:48 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase, firestore]
---

문서 삭제를 먼저 알아보자. 귀찮으니까 MyGet 클래스에 몰아넣을 것이다.

```dart

class MyGet extends HookWidget {
  const MyGet({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final myDataList = useState<List<MyData>>([]);

    // get 함수 생략

    final useOnFocused = useState("");

    myFireStoreDelete() async {
      final db = FirebaseFirestore.instance;
      CollectionReference myRef = db.collection("users");

      QuerySnapshot querySnapshot =
          await myRef.where("name", isEqualTo: useOnFocused.value).get();
      if (querySnapshot.docs.isNotEmpty) {
        for (QueryDocumentSnapshot documentSnapshot in querySnapshot.docs) {
          await documentSnapshot.reference.delete();
        }
      } else {}
    }

    return myDataList.value.isEmpty
        ? const Center(child: CircularProgressIndicator())
        : ListView.builder(
            itemCount: myDataList.value.length,
            itemBuilder: (BuildContext context, int index) {
              return Row(
                children: [
                  Expanded(
                    flex: 3,
                    child: ListTile(
                      title: Text(myDataList.value[index].name),
                      subtitle: Text(myDataList.value[index].age.toString()),
                    ),
                  ),
                  Expanded(
                    child: IconButton(
                      onPressed: () {
                        useOnFocused.value = myDataList.value[index].name;
                        myFireStoreDelete();
                      },
                      icon: const Icon(Icons.delete),
                    ),
                  ),
                ],
              );
            },
          );
  }
}
```

일단 Row로 만들고 삭제 아이콘을 추가한다. 버튼 효과를 만들고, useOnFocused.value에 값을 대입한다.
collection을 받아오는 것까진 get함수와 동일하다.
여기서부터는 where문을 사용하는데, name 필드에 있는 값이 useOnFocused.value와 같은 값을 찾아내는 것이다.
if문에서는 QuerySnapshot에 포함된 결과가 하나 이상일 경우를 찾고,
for문에서는 해당 name의 모든 문서를 삭제한다. 보통은 name의 값은 같을 수도 있어서 고유한 Primary Key와 같은 값을 찾는 것이 좋다.

우리는 이미 Get 함수에서 자동 갱신 시스템을 만들어뒀기에, 삭제하면 곧바로 갱신되는 것을 볼 수 있다.

업데이트의 경우 저기서 delete 부분에 update를 넣고, 해당하는 Map을 매핑하면 된다.
```dart
await documentSnapshot.reference.update({'name':name, 'age': age, 'id':id});
```
이런 식으로 말이다.

---