### 🔥 클라우드의 이해 1, 2 복습하기

![](https://velog.velcdn.com/images/eugenieseo16/post/df67a717-662c-422e-8507-9ab6df4c077a/image.jpg)

### 🔥 도커란?

<img src="https://velog.velcdn.com/images/eugenieseo16/post/61f30743-7af0-480f-b35f-298cebe1818e/image.png" title="" alt="" width="373">

#### ⭐ **컨테이너 기반의 오픈소스 가상화 플랫폼**

dotCloud의 창업자인 Solomon Hykes가 2013년 3월 Pycon Conference에서 발표한 컨테이너 기반의 오픈소스 가상화 플랫폼

- 리눅스 컨테이너에 리눅스 어플리케이션을 프로세스 격리기술을 사용하여 쉽게 컨테이너로 실행하고 관리할 수 있게 하는 기술.
- 컨테이너 기술 사용을 위해, 컨테이너를 구동할 수 있는 환경이 설치되어야 하는데, 구동을 시켜주는 대표적인 기술이 도커이다. 원래는 회사명이었지만 현재는 일반명사로 쓰인다. Docker만 설치되어 있다면 Spring, Nodejs 등 OS와 CPU에 관련 없이 컨테이너에만 넣으면 모두 동일한 환경에서의 실행이 가능하다. 배포 및 관리를 단순화할 수 있다.

** ➡ <span style="background-color:#e2f0d9;">  컨테이너를 만들고, 배포하고, 구동한다.</span> **
![](https://velog.velcdn.com/images/eugenieseo16/post/2b020237-4394-4365-b238-107d541a39f1/image.png)

컨터이너를 만들기 위해서는 도커파일을 만들고, 도커파일을 이용해서 도커 이미지를 만들어야한다. 이를 이용하여 컨테이너를 구동할 수있다.

#### ⭐ 도커파일

<img src="https://velog.velcdn.com/images/eugenieseo16/post/b206b530-434b-4b27-8093-9a89e286310b/image.png" title="" alt="" width="178">
도커에서 이미지를 생성하기 위한 용도로 작성하는 파일로 만들 이미지에 대한 정보를 기술해 둔 템플릿이라고 보면 된다. 코드 형태의 텍스트 문서로, 여러가지 지시어를 사용하여 이미지를 제작할 수 있다.

✅ 어플리케이션을 구동하기 위한 필수 구동 파일
✅ 어떤 라이브러리를 설치해야하는지, 외부 dependencies
✅ 필요한 환경변수
등등을 포함할 수 있다.

#### ⭐ 도커이미지

<img src="https://velog.velcdn.com/images/eugenieseo16/post/b8f68db1-eff9-4636-a1f2-8b54068a280e/image.png" title="" alt="" width="274">

도커는 서비스 운영에 필요한 서버 프로그램, 코드 및 라이브러리, 컴파일된 실행 파일 등을 포장하고, 전송하기 위해 **"docker image"**를 사용한다. 
<span style="background-color:#fff5d8;"> **도커이미지**는 **컨테이너 생성과 실행에 필요한 모든 파일과 환경을 가진 것**</span>으로, 실행 환경을 맞추기위해 다른것을 설치할 필요가 없는 상태의 파일을 뜻한다. 도커이미지만 있다면 이미지를 다운받고 컨테이너를 생성만 하면된다. 도커 이미지는 Docker file이라는 파일형태로 만들어지는데, 도커파일에는 소스코드와 함께 의존성 패키지 등 사용한 설정 파일을 관리하기 쉽도록 명시되어있다.
변경이 불가능한 불변의 상태로 볼 수있다.

![](https://velog.velcdn.com/images/eugenieseo16/post/8bf8cbf8-9b70-4790-9adf-991c7e1ad186/image.png)

### 🔥 도커 설치 및 실행하기

1. 도커 다운로드
   [도커 사이트](https://www.docker.com/)에 들어가서 각 운영체제에 맞는 도커를 다운받는다.
   ![](https://velog.velcdn.com/images/eugenieseo16/post/02e1298f-8b82-48c3-a8a5-1168b30d809a/image.gif)
   ✅ VScode를 사용한다면, [docker 확장프로그램](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)도 설치한다.(도커의 문법 등 도커 파일 생성 시 도움을 받을 수 있는 확장 프로그램이다.
   ![](https://velog.velcdn.com/images/eugenieseo16/post/8dad9dc7-c8ac-4744-9525-f48f437df43c/image.png)

> 참고:
> [초보를 위한 도커 안내서 - 도커란 무엇인가?](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
> [code-lab1.tistory](https://code-lab1.tistory.com/236)
> [han-py.tistory](https://han-py.tistory.com/494)
> [sunrise-min.tistory](https://sunrise-min.tistory.com/entry/Docker-Container%EC%99%80-Image%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)
> [드림코딩 - 도커 한방에 정리](https://www.youtube.com/watch?v=LXJhA3VWXFA)
