---
title: Flutter - Flutter SDK checkout Error
date: 2024-01-13 14:25:00 +/-09:00
category: [flutter/error]
tag: [flutter, error]
---

```terminal
! Warning: `flutter` on your path resolves to {경로}, which is not inside your current Flutter SDK checkout at
      /Users/shech/desktop/fluttersdk/flutter. Consider adding {경로} to the front of your path.
    ! Warning: `dart` on your path resolves to {경로}/dart, which is not inside your current Flutter SDK checkout at
      /Users/shech/desktop/fluttersdk/flutter. Consider adding {경로}/bin to the front of your path.
```
가 뜰 적이 있다. 당황하지 말자.

1. 
```terminal
brew uninstall dart
```
Flutter를 쓸 적에 dart가 필요 없으니 brew에서 지운다.

2. 이런데도 안 되는 경우가 종종 있다. 환경변수에서 Dart가 남아있기 때문이다.
```terminal
nano ~/.zshrc
```

들어가서 이 내용을 지워준다.
```terminal
"$HOME/.pub-cache/bin"
```

---