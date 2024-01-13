---
title: python - pip install error(pip 설치 오류)
date: 2022-9-22 12:39:26 +/-09:00
category: [python/discord]
tag: [python, discord]
---

<b>'pip3'은(는) 내부 또는 외부 명령, 실행할 수 있는 프로그램, 또는
배치 파일이 아닙니다.</b>
라는 메시지가 뜰 때마다 내 마음은 철렁하게 된다.
그럴 때 해결하는 방법들을 찾아보자.

1. python -m ensure
파이썬 초기 버전 pip로 되돌린다. 알다시피 3.4 이상부터는 pip가 내장되어 있기 때문에 이를 이용하는 것.
간혹 이마저도 먹히지 않는 경우가 있는데, 오류가 나면서 뜨는 링크의 폴더로 들어가서 접두사가 특수문자인 파일들을 전부 지워주면 된다. ,<b>단, 접두사 언더바 파일이나 폴더의 경우 지우지 말아야 한다.</b>
python -m ensure --upgrade를 넣으면 최신 버전으로 업그레이드도 한다.
c:\users\사용자이름\appdata\local\programs\python\python38\lib\site-packages
오류가 나면 들어가는 폴더는 위와 같다.

2. 시스템 환경 변수 편집(공식 제안)
시스템 환경 변수 편집 - 환경 변수에서 Path 항목에서
C:\Users\사용자이름\AppData\Local\Programs\Python\Python310
C:\Users\사용자이름\AppData\Local\Programs\Python\Python310\Scripts
이 둘을 추가한다.

3. pip 수동 설치
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
위 두 문장을 입력한다.

4. 셋업(Setup)

![python-error-1.png](/assets/postingImage/python-error-1.png)

말 그대로 재설치를 한다. 수정/수리가 안 되면 삭제 후 재설치를 진행하는데,

![python-error-2.png](/assets/postingImage/python-error-2.png)

밑에 있는 Add Python 버전이름 to PATH 를 반드시 체크해줘야만 한다.

---