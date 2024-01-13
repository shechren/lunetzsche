---
title: Flutter - 파이어스토어(Firestore) 설정하기
date: 2023-08-18 11:33:17 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase, firestore]
---

파이어베이스를 등록했으니 이제 FireStore에서 Firestore Database와 Firestore Storage를 연동해보자.

Cloud FireStore를 만든다.
그리고 Storage를 만든다.
둘 모두 규칙에 들어간다.
allow read, write: if false;로 나오는데, false를 true로 바꿔준다.
이러면 읽기와 쓰기를 모두 할 수 있게 된다.

다음은 코드를 볼 시간이다.
당연하지만 파이어베이스에 먼저 initialise를 해야 한다.

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(MyApp());
}
```
여기까지의 설정은 앞서서 이미 서술했기에 추가로 설명하지 않는다.
이제 새로 나온 Hooks라는 신기술을 다시 사용해보자. Stateless와 Stateful로 나누는 구닥다리 기술은 이제 더이상 쓰고 싶지 않다.

```dart
class MyApp extends HookWidget {
  MyApp({super.key});


  @override
  Widget build(BuildContext context) {
    var useName = useState('');
    var useAge = useState(0);
    final useFormKey = useMemoized(() => GlobalKey<FormState>());
    
    return MaterialApp(
        home: Scaffold(
            body: Center(
      child: Form(
        key: useFormKey,
        child: Column(
          children: [
            TextFormField(onSaved: (String? value) {
              if (value != null && value.isNotEmpty) {
                useName.value = value;
              }
            }),
            const SizedBox(height: 10),
            TextFormField(
                keyboardType: TextInputType.number,
                onSaved: (String? value) {
                  if (value != null && value.isNotEmpty) {
                    useAge.value = int.parse(value);
                  }
                }),
            ElevatedButton(onPressed: myFireStoreAdd, child: const Text("저장"))
          ],
        ),
      ),
    )));
  }
}
```

var useName과 var useAge에 값을 넣을 것이다.
key는 useMemoized()로 받아 글로벌 키를 선언해준다.
텍스트 폼을 열고, 검사해서 useName과 useAge의 값을 바꿔준다.
마지막으로, ElevatedButton을 선언해서 함수를 만든다.
```dart
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
    print("Firestore와의 연결이 실패했습니다: $e");
  }
}
```

빌드 함수 내에 있는 버튼 함수다.
db라는 이름으로 생성자를 만들어주고, Map 타입으로 name과 age를 한 곳에 넣어준다.
즉, users는 테이블 이름과 같고, name과 age는 컬럼과 같은 것이다.
우리는 이것을 json 형식으로 신나게 꺼내 쓸 수 있다. 저장을 json형식으로 했으니까 말이다.

마지막으로 우리는 문서의 식별자를 지정해주는데, 보통은 나이보다는 이름으로 꺼내기 마련이니까 name을 식별자로 지정해준다. 그것이 바로 await문이다.

이제 데이터를 입력해서 저장하고, 데이터베이스로 가서 확인해보면 된다.

![flutter-firebase-2.jpg](/assets/postingImage/flutter-firebase-2.jpg)

---