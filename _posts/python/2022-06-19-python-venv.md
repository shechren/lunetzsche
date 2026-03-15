---
title: Python - 가상환경 만들기
date: 2022-06-19 18:14:20 +/-09:00
category: [python/basic]
tag: [python, venv]
---

파이참을 실행하고 새 프로젝트를 연다.

![python-venv1.png](/assets/postingImage/python-venv1.png)


이미 콘다가 설치되어 있어 콘다를 써도 되지만, 우리는 새로운 폴더를 만들어 cmd 또는 터미널로 실행시킬 것이다.
Scraping 폴더를 만든다. 프로젝트 생성이 끝나면 파이참을 닫는다.

cmd 또는 터미널에서 cd 명령어를 통해 접근하고자 하는 폴더 바로 위까지 접근하고
python -m venv 폴더이름
을 입력한다.

그다음 해당 폴더에 가서 Scripts 폴더로 가고
activate를 치면 가상환경에 진입한다. 비활성화는 deactivate다.

---