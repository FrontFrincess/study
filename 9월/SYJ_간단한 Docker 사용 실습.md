### 🔥 도커 설치 및 실행하기
#### 1. 도커 다운로드
[도커 사이트](https://www.docker.com/)에 들어가서 각 운영체제에 맞는 도커를 다운받는다.
![](https://velog.velcdn.com/images/eugenieseo16/post/02e1298f-8b82-48c3-a8a5-1168b30d809a/image.gif)
✅ VScode를 사용한다면, [docker 확장프로그램](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)도 설치한다.(도커의 문법 등 도커 파일 생성 시 도움을 받을 수 있는 확장 프로그램이다.
![](https://velog.velcdn.com/images/eugenieseo16/post/8dad9dc7-c8ac-4744-9525-f48f437df43c/image.png)

도커를 설치하다가 오류가 발생했다. 전에 도커를 설치한 적이 있어서 생긴 문제같아 로컬디스크에서 도커와 관련된 파일들을 삭제하고 재설치 했더니 문제가 해결되었다.

---
하지만 도커를 설치한 후, 실행을 했더니 다른 문제가 발생했다.
![](https://velog.velcdn.com/images/eugenieseo16/post/15495989-b5f4-4b92-b77a-f605d820be83/image.JPG)
**WSL**은 <span style="background-color:#fff5d8;">**Windows Subsystem for Linux**</span>의 약자로 Windows10, Windows11 환경에서 리눅스를 사용할 수 있게하는 시스템이다.

WSL은 1과 2버전이 있는데, 둘은 많은 차이가 있다.
**WSL1**은 Windows의 NT Kernel 위에 WSL을 올리고 리눅스용 어플리케이션을 돌리는 방식이고, **WSL2**은 Hypervisor 위에 윈도우 NT 커널과 리눅스 커널을 각각 올리는 방식이다. 자세한 차이는 마이크로소프트의 [[WSL 1과 WSL 2 비교]](https://learn.microsoft.com/ko-kr/windows/wsl/compare-versions#comparing-features)에서 확인 할 수 있다.

경고문은 WSL1을 WSL2로 업데이트해야 도커를 사용할 수 있다는 뜻 같아, 나와있는대로 Powelshell 관리자 모드에 들어가 ```
wsl --update``` 를 입력했다. 하지만 계속해서 "잘못된 명령줄 옵션입니다." 라는 오류가 떴고, 이는 ```wsl --install```을 입력했을 때도, 마찬가지였다. 결국, [경고문 속 공식문서](https://docs.microsoft.com/windows/wsl/wsl2-kernel)로 들어가 방법을 찾기로 했다.

방법은 잘 정리되어있기에 들어가서 따라하기만 하면 된다. 
간단하게 정리하자면 다음과 같다.
1. <span style="background-color:#fff5d8;">**Windows 로고 + R을 누른 뒤 winver를 입력, 윈도우 정보를 확인한다.** </span>
![](https://velog.velcdn.com/images/eugenieseo16/post/f4745f73-cc4e-4338-9765-ba066f1b3eb4/image.png)


이것 보다 낮은 빌드는 WSL 2를 지원하지 않기 때문에 [Windows 버전을 업데이트](https://www.microsoft.com/ko-kr/software-download/windows10) 해줘야한다. 
2. 사양과 버전에 문제가 없다면 <span style="background-color:#fff5d8;">Powershell을 관리자 모드로 키고, 다음 명령어들을 입력해 준다.</span>
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestar
```
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
각각은 Linux용 Windows 하위 시스템 옵션 기능과 Virtual Machine 플랫폼 옵션 기능을 사용하도록 설정하는 명령어 이다. 이상 없이  완료되었다면 옵션 기능 설정과 WSL 설치를 완료하기 위해 컴퓨터를 재부팅 해준다.

3.<span style="background-color:#fff5d8;"> Linux 커널 업데이트 패키지를 다운로드한다.</span>
[x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) 를 클릭하여 최신 패키지를 다운받을 수 있다. 

4.<span style="background-color:#fff5d8;"> WSL 2를 기본 버전으로 설정한다.</span>
Powershell에 ```wsl --set-default-version 2``` 를 입력하여 WSL2를 기본버전으로 설정한다. 
 
5.<span style="background-color:#fff5d8;"> [Microsoft Store](https://apps.microsoft.com/store/apps)에 들어가 원하는 Linux 배포 프로그램을 다운받고, 실행시키고, 계정을 만든다.</span>
나는 [Ubuntu 22.04 LTS](https://apps.microsoft.com/store/detail/ubuntu-22042-lts/9PN20MSR04DW?hl=ko-kr&gl=kr)를 다운받았다.
![](https://velog.velcdn.com/images/eugenieseo16/post/90e860e0-b7d9-44ee-b849-3e3c351b48d9/image.png)
원하는 아이디와 비밀번호를 입력하여 우분투 계정을 만든다.
![](https://velog.velcdn.com/images/eugenieseo16/post/00d6181b-9435-4a0d-a662-372ffa4ec700/image.png)

이후, ```sudo apt update``` 를 이용하여  업데이트 파일을 확인하고, 비밀번호 입력 후 다시한번 ```sudo apt update``` 명령어로 우분투를 최신으로 업그레이드하고, [Y/n] 질문이 나오면 Y를 입력해 업그레이드를 완료한다!

여기까지 했다면, 도커가 실행이 되어야할텐데!! 몇번 재부팅을 해도 안되어서 윈도우 업그레이드를 다시한번 확인하러갔더니 "윈도우 업데이트 문제가 발생했습니다 나중에 설정을 다시 열어보세요" 라는 오류화면만 나왔다.
![](https://velog.velcdn.com/images/eugenieseo16/post/a1b4ee21-fcf9-47fe-a1b6-44264fe3dcf7/image.png)
이에, 또 재부팅을 몇번하고 [마이크로소프트의 공식 답변](https://answers.microsoft.com/ko-kr/windows/forum/all/windows/947c3b32-a277-40eb-8053-ab7dcc7a7e10)대로 해보았지만, 되지 않았다. 그래서 [수동으로 업데이트](https://www.microsoft.com/en-us/software-download/windows10)를 해주었다. 그래도 안되었지만 이상하게 하루지나니까 갑자기 도커가 잘 실행되었다....

---
이제 진짜 실습을 할수있다....!
#### ⭐ node.js 를 이용한 간단한 실습
🌜 <span style="background-color:#fff5d8;">**프로젝트 만들기**</span>
1. ```npm init -y``` 명령어를 사용하여 프로젝트를 생성한다.
![](https://velog.velcdn.com/images/eugenieseo16/post/23f67ccc-8683-4736-8d0c-da6739fde7a5/image.png)
하...이젠 뭐 이런거에도 에러가뜬다ㅎㅎ침착하게 node.js와 npm을 제거하고, 다시 설치해준다.
![](https://velog.velcdn.com/images/eugenieseo16/post/b0fefbac-c594-4005-9e54-c72c6bfab38a/image.png)
역시 뭐가 안될때는 삭제하고 다시 설치하면 다 된다.
2. ```npm i express``` 로 백엔드 만들고, 프로젝트 초기화!
![](https://velog.velcdn.com/images/eugenieseo16/post/d3a22df7-561b-47e5-aebd-7cd431a59348/image.png)
3. index.js 파일 만들고, 요청이 오면 '간단한 도커실습'을 띄우는 코드를 입력
![](https://velog.velcdn.com/images/eugenieseo16/post/1d598166-f48c-470e-855a-c49b87bc9ebe/image.png)

```
    const express = require("express");
    // express() 함수를 호출하여 Express 애플리케이션 인스턴스를 생성
    const app = express();
    // 사용자가 루트 경로로 접속하면 "🌱간단한 도커실습🌱"라는 텍스트를 응답
    app.get("/", (req, res) => {res.send("🌱간단한 도커실습🌱");});
```
이후 node.js 실행 시키고, 8080으로 접속하면 간단한 도커실습이 떠있다.
![](https://velog.velcdn.com/images/eugenieseo16/post/2a58a4b3-077c-44f8-8c47-fe68fe64e31f/image.png)![](https://velog.velcdn.com/images/eugenieseo16/post/75d2aa51-371a-40ff-951e-82996c1c1ac9/image.png)

🌜 <span style="background-color:#fff5d8;">**이제 진짜 진짜 실습가능**</span>
1. Dockerfile 생성
도커 파일은 어떤 이미지를 만들건지, 우리 프로젝트의 어떤 것들이 필요한지 명시하는 파일이다. Dockerfile을 만든 뒤, 우리 파일에 대해 알려주는 명령어를 작성한다. 명령어에 대한 자세한 설명은 [도커 공식문서](https://docs.docker.com/engine/reference/builder/) 에서 찾아보면 된다.
![](https://velog.velcdn.com/images/eugenieseo16/post/555cd493-7459-4373-88ab-6324edce8f83/image.png)

제일 처음에는 항상 ```FROM baseImage```로 시작한다. 노드 같은 경우는 노드에서 미리 만들어둔 이미지가 있기 때문에 그걸 이용한다. 18은 노드의 버전을 의미하고 alpine은 가장 작은 단위로 정말 필요한 것들만 담겨져 있는 이미지를 뜻한다.

```
# 제일 처음에는 항상 FROM baseImage
# 노드 같은 경우는 노드에서 미리 만들어둔 이미지가 있다.
FROM node:18-alpine

# 어떤 경로, 디렉토리에 있는 어플리케이션을 복사해 올것인지
WORKDIR /app

# 빈번하게 변경되는 파일일수록 제일 나중에 작성하는게 좋다.
# 명령어 하나하나가 레이어로 되어있어서 나중에 작성되는게 레이어 제일 위쪽에 위치하게 된다.
# 변경사항이 발생하면 발생한 레이어부터 최상단의 레이어까지를 업데이트한다.
COPY package.json  package-lock.json ./

# npm install은 버전을 정확하게 맞추지는 않음. ci는 정확하게 버전을 설치함
RUN npm ci

COPY index.js .

ENTRYPOINT [ "node", "index.js" ]

```
파일을 다 작성했다면, 이미지를 만든다.

2. Docker 이미지 생성

터미널에 ```docker build -f Dockerfile -t seo-docker .``` 명령어를 입력한다. 명령어에 해당하는 설명은 다음과 같고, 명령어에 대한 자세한 설명은 공식문서에서 확인할 수있다.
![](https://velog.velcdn.com/images/eugenieseo16/post/c30ad4c6-b11c-4e44-b394-a1901adf020b/image.jpg)

도커이미지를 만든 후, ```docker images``` 명령어를 통해 만들어진 이미지를 확인할 수 있다.
![](https://velog.velcdn.com/images/eugenieseo16/post/34d4a763-4188-4c03-88ec-e9306e2717f4/image.png)

3. Docker 컨테이너 실행
```docker run -d -p 8080:8080 seo-docker``` 명령어를 통해 컨테이너를 실행한다.
![](https://velog.velcdn.com/images/eugenieseo16/post/0f722477-5e2d-427d-93cd-35213b5867ef/image.jpg)

![](https://velog.velcdn.com/images/eugenieseo16/post/186742fc-0e61-4492-a118-be14479e14b3/image.png)
```docker ps``` : 현재 실행중인 컨테이너들의 리스트를 확인.
```docker logs 컨테이너아이디``` : 컨테이너 아이디를 갖는 컨테이너의 정보를 확인 

3. [Dockerhub](https://hub.docker.com/)를 이용한 Docker 이미지 배포
[Dockerhub](https://hub.docker.com/)에 들어가 Repository를 생성한다.
![](https://velog.velcdn.com/images/eugenieseo16/post/35c6b52d-5932-4a5c-9927-262d700290e7/image.gif)
Repository 이름을 지정하고 Create한다. 도커허브에서는 image의 이름과 Namespace/repository의  이름이 매칭되어야한다. image의 이름을 바꾸고 싶다면 ```docker tag``` 명령어를 사용하면 된다.
![](https://velog.velcdn.com/images/eugenieseo16/post/2653d657-2493-4f09-8ca2-6ee27277b8ec/image.png)
![](https://velog.velcdn.com/images/eugenieseo16/post/f1eca03b-bd08-44a5-af27-ea546962de89/image.png)

```docker login``` 명령어를 통해  도커 계정로그인을 하고, 레포지토리를 생성했을 때 화면에 나온 명령어인 ```docker push eugenieseo/seo-docker:latest```를 입력해준다.
![](https://velog.velcdn.com/images/eugenieseo16/post/df0eedbb-ec8d-4111-aa6e-8cd753b17f8e/image.png)
![](https://velog.velcdn.com/images/eugenieseo16/post/82b239c0-41b1-4246-8d87-cd002bdb67ff/image.png)

도커 허브로 가서 등록된 이미지를 확인한다.
![](https://velog.velcdn.com/images/eugenieseo16/post/749bb294-88bf-4989-b2ba-ca48b68f306f/image.png)

드디어 실습 끝!

> 참고 : [도커 한방에 정리 🐳 (모든 개발자들이 배워보고 싶어 하는 툴!) + 실습](https://www.youtube.com/watch?v=LXJhA3VWXFA)
