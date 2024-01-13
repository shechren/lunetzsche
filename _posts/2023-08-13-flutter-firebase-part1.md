---
title: Flutter - 파이어베이스(Firebase) 1
date: 2023-08-13 15:00:20 +/-09:00
category: [flutter/firebase]
tag: [flutter, firebase]
---

firebase는 쉽게 생각하면 백엔드 서비스를 쉽게 제공해주는 곳이다.
일단 설치에 Node.js가 필요하고, 프로젝트 추가도 알아서 할 것이라 믿는다. 알려주는 곳도 많지만, 그냥 공식 레퍼런스를 따라도 너무나도 쉽게 설치가 된다.
[!https://console.firebase.google.com/?hl=ko](https://console.firebase.google.com/?hl=ko)

다만 로그인을 할 적에 문제가 되는데, Mac은 괜찮은데 **Windows**가 문제다
PowerShell의 권한 문제 때문인데, 제일 먼저 PowerShell을 관리자 권한으로 연다.

```terminal
get-ExecutionPolicy
```
위 명령어를 치면 Restricted라고 나올 텐데, 권한에 제약이 있는 상태다.

```terminal
set-ExecutionPolicy
```
이후 Y를 눌러 권한을 승인해주면 이제 PowerShell에서도 자유롭게 firebase에 접근할 수 있다.

또한 중요한 건 환경변수에 정확한 Path를 적어줘야 한다는 점이다.

![flutter-firebase-1.jpg](/assets/postingImage/flutter-firebase-1.jpg)

자 이제 명령어를 실행할 차례다.

```terminal
firebase login --interactive
```

---