---
title: Flutter - Radio in Dialog
date: 2024-01-22 21:48:00 +/-09:00
category: [flutter/state]
tag: [flutter, hooks, radio, dialog]
---

flutter_hooks를 쓰면 왜 Dialog() 위젯에는 Provider가 적용되지 않을까 의문이 있었다.
그리고 그것에 대한 해답을 이제서야 찾아냈다.

```dart
Widget showedList() {
  return MultiProvider(
    providers: [
      ChangeNotifierProvider(
        create: (context) => PostingProvider(),
      ),
      ChangeNotifierProvider(
        create: (context) => ProfileProvider(),
      ),
      ChangeNotifierProvider(
        create: (context) => LoginProvider(),
      )
    ],
    child: HookBuilder(
      builder: (context) {
        final innerPostingProvider = Provider.of<PostingProvider>(context);
        final innerProfileProvider = Provider.of<ProfileProvider>(context);
        final innerLoginProvider = Provider.of<LoginProvider>(context);
        return SimpleDialog(
          title: Text('카테고리를 선택하세요.'),
          children: [
            SizedBox(
              width: 400,
              height: 400,
              child: FutureBuilder(
                future: innerProfileProvider.loadProfile('user', innerLoginProvider.user!.email!),
                builder: (context, snapshot) {
                  return ListView.separated(
                    shrinkWrap: true,
                    itemBuilder: (context, index) {
                      innerPostingProvider.selectedValue ??= 0;
                      return RadioListTile(
                        title: innerProfileProvider.specific != null &&
                            innerProfileProvider.specific!.isNotEmpty
                            ? Text(innerPostingProvider.specialList[index])
                            : Text(innerPostingProvider.categoryList[index]),
                        value: index,
                        groupValue: innerPostingProvider.selectedValue,
                        onChanged: (value) {
                          innerPostingProvider.changeSelectedValue(value); // 선택한 값을 저장
                          if (innerProfileProvider.specific != null &&
                              innerProfileProvider.specific!.isNotEmpty) {
                            categoryConverter(value);
                          } else {
                            specificConverter(value);
                          }
                        },
                      );
                    },
                    separatorBuilder: (context, index) {
                      return const Divider(color: Colors.black, thickness: 0.8);
                    },
                    itemCount: innerProfileProvider.specific != null &&
                        innerProfileProvider.specific!.isNotEmpty
                        ? innerPostingProvider.specialList.length
                        : innerPostingProvider.categoryList.length,
                  );
                }
              ),
            ),
          ],
        );
      }
    ),
  );
}
```

Dialog()들 또한 하나의 Scaffold 비스무리한 위젯 취급이므로 Provider()를 새로이 설정해줘야 한다.

---