---
title: aws - 디스코드 봇을 AWS EC2로 호스팅하기
date: 2022-12-04 14:12:26 +/-09:00
category: [aws/ec2]
tag: [ec2, aws, discord]
---

먼저 aws에 가서 회원가입을 한 뒤 EC2에 들어간다.
우분투를 고른 뒤 적당히 다 스킵해서 인스턴스를 만든다.
아, pem키를 받는 것도 잊지 말아야 한다.

pem키가 저장된 곳에 가서 아래 명령어를 친다.

![aws-discord-1.png](/assets/postingImage/aws-discord-1.png)

icacls.exe 키이름.pem /reset
icacls.exe 키이름.pem /grant:r %username%:(R)
icacls.exe 키이름.pem /inheritance:r

![aws-discord-2.png](/assets/postingImage/aws-discord-2.png)

WinSCP를 열고, 로그인을 한다. linux의 경우 사용자 아이디는 ec2-user, 우분투는 ubuntu다.
호스트 아이디는 자신의 AWS 인스턴스에서 퍼블릭 IPv4 주소를 받아온다.
비밀번호 초기값은 없으며, 포트 초기값은 22다.
비밀번호에서 고급으로 들어가 위 사진처럼 간다. 개인 키 파일 불러오기에서 pem을 putty값으로 변경할 필요성이 있다.

![aws-discord-3.png](/assets/postingImage/aws-discord-3.png)

파일들을 업로드한다.

이제 cmd를 연다

마지막으로 cd 경로를 통해 자신이 등록한 디렉토리에서
ssh -i pem이름.pem ubuntu@퍼블릭IPv4

를 입력하면 서버에 들어갈 수 있다.
(매번 이걸로 로그인해야 하니 텍스트를 저장해두자.)

리눅스에서는
sudo yum(레드햇 기반)

우분투에서는
sudo apt이 기본 명령이다(sudo는 가상환경을 연다는 뜻. 관리자 권한과 거의 동급이니 조심하자.)

sudo apt-get update

sudo apt-get upgrade

이 둘을 먼저 해준다.

sudo apt-get install 을 통해 파이썬과 pip와 오만가지 모듈들을 설치해준다.

sudo apt python3-pip 등등...

pip install -r requirements.txt로 한번에 설치도 가능하다.


서버에서는 python 버전을 맞춰준 뒤, python3 봇이름(대소문자 주의).py를 실행하면 된다.

screen 이후 python3 봇이름.py를 하면 봇을 서버에서 구동시킨다.

screen -r로 스크린을 돌아갈 수 있다.

screen -ls로 열린 스크린을 조회할 수 있다.

스크린에서 나오는 단축키는 Ctrl A D다.

강제 종료는 exit(), netstat -ntlp로 구동중인 스크린을 kill 스크린이름 이런 식으로 정리할 수도 있다.

---