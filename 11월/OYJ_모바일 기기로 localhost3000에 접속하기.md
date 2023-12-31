# 모바일 기기로 localhost:3000에 접속하기

![post-thumbnail](https://velog.velcdn.com/images/yjohbjects/post/e7af27ec-8366-4a26-b63f-7716d055cabe/image.png)

개발을 진행할 때, 여러 기기에서의 화면을 보면서 개발을 해야할 상황이 많다.
본인의 경우는 모바일 기기와 태블릿을 켜두고 개발한 경험이 있는데, 이때 로컬호스트를 다른 디바이스에서 열어서 실시간으로 변화하는 모습을 보면서 개발하는 것이 편리했다.

우선 다른 기기에서 로컬을 접속할 수 있는 방법은 두가지를 소개할 것이다.
`1. 컴퓨터와 같은 와이파이 접속`
`2. ngrok.exe`
두가지 방법 모두 쉽고 간단하니 걱정 노노!

**우선 시작하기 앞서, 두 가지 방법 모두 로컬에서 프로젝트를 구동시켜야한다! (`npm start`)**


## 1. 컴퓨터와 같은 와이파이 접속
`준비물`: 구동되고있는 로컬호스트, 컴퓨터 ip 주소

1. 말 그대로 로컬 화면을 보고싶은 기기로 컴퓨터가 접속한 와이파이와 동일한 와이파이로 접속
2. 크롬, 사파리, 등 원하는 웹 브라우저를 연다
3. 웹 브라우저 주소창에 <ip_address:port>를 입력
예시) 111.22.00.33:3000
4. 완성

## 2. ngrok.exe
`준비물`: 구동되고있는 로컬호스트, ngrok

1. [ngrok 설치창](https://ngrok.com/download)에서 사양에 맞추어 설치
2. 다운받은 파일의 압축을 해제하고 ngrok.exe를 `관리자 권한으로 실행`
3. 아래 command를 입력
```bash
ngrok.exe http 3000
```
4. 세션이 online으로 뜨면 Forwarding에 뜨는 https:// 주소를 통해 모바일 기기에서 접속
5. 완성

정말 간단한 방법으로 다른 기기에서 로컬화면을 볼 수 있게 되었다.
와이파이 환경이 따라준다면 같은 와이파이 접속으로 가능하겠지만, 그런 상황이 따라주지 않을 경우 ngrok이라는 무료 프로그램을 통해서도 가능하다.
이를 통해서 조금은 더 마음이 시원해졌길(?) 바란다!