# [FE] Netlify로 React 프로젝트 빠르게 배포하기



리액트 프로젝트를 쉽고 간단하고 매우 빠르게 netlify로 배포 해보자!

### 잠깐! Netlify가 뭐죠?
Netlify는 정적 웹 호스팅 및 웹 개발 서비스를 제공하는 플랫폼이다. 더 간단히 말하면 정적 사이트를 손쉽게 배포할 수 있는 곳!이랄까.
![](https://velog.velcdn.com/images/somda/post/22b249c9-9a80-447e-9343-b6cd71f474b0/image.png)
무엇보다 100GB용량에 빌드 300분까지 무료로 사용 가능하고, SSL 인증서를 자동으로 제공하기까지 하는 데다 CI/CD도 구축할 수 있다! 
프엔 개발 취준생에게 빛과 소금...✨
유사한 서비스로는 vercel이 있다. vercel도 매우매우 간단하다고 하니 입맛에 맞는 걸로 골라잡으면 되겠다.


## Netlify로 배포하기
### 회원 가입 및 팀 생성
최초로 회원 가입을 하고 팀을 생성하면 아래와 같은 창을 볼 수 있다.
(github 계정과 연동해 회원가입을 했다.)
![](https://velog.velcdn.com/images/somda/post/b3ee6fe9-4cf9-4b46-a1b3-c6fb1e3a1046/image.png)

### Github 연동
`Import from Git`을 눌러 Github 레포와 연동할 수 있다.
![](https://velog.velcdn.com/images/somda/post/65e5609e-f94a-4c23-89af-cbd43bb24f9c/image.png)
Authorize 해주고,
![](https://velog.velcdn.com/images/somda/post/72e4f507-4aca-48c7-be4f-5f52a18539b7/image.png)
Configure하면
아래과 같이 위치를 지정할 수 있다.
![](https://velog.velcdn.com/images/somda/post/ae8259a6-2005-44eb-88fa-5881b25d0ab7/image.png)
개인 프로젝트를 배포할 예정이라 github 아이디 클릭!
![](https://velog.velcdn.com/images/somda/post/dc51961c-aa8b-4966-8b8c-877227fe1b0d/image.png)
특정 레포만 배포할 권한을 줄 지, 아니면 모든 레포에 줄 지 정할 수 있다.
개인 과제 링크를 배포할 예정이라 특정 레포만으로 생성했는데,
전체 레포를 해도 상관 없다. (추후에 설정 변경 가능!)
그럼 `Install`까지 해주자
![](https://velog.velcdn.com/images/somda/post/bd82e961-e4c1-432e-ad18-b6be50d35a18/image.png)
문제 없이 등록된 모습을 볼 수 있다!

### 배포하기
이제 배포할 레포를 클릭!
![](https://velog.velcdn.com/images/somda/post/fb4125d8-ca93-4986-ac36-032c21cbfda6/image.png)
어떤 브랜치를 기준으로 배포할지 설정할 수 있다.
나는 master로 설정했다.
![](https://velog.velcdn.com/images/somda/post/7f431cd6-241c-4901-9f8d-6f2e21b69a2c/image.png)
Base directory
* 레포 안에서 백, 프론트가 나뉠 경우 그 중 하나의 폴더명 입력하면 된다. 현재 케이스에선 레포가 프론트 하나로만 되어있어서 입력하지 않고 넘어간다.
Build Command
* npm이라면 CI= npm run build, yarn 이라면 CI= yarn build를 입력 
앞에 CI=을 붙이지 않으면
![](https://velog.velcdn.com/images/somda/post/c5ab03a0-3340-413e-ace5-bcc3b0846d19/image.png)
이렇게 오류`Build script returned non-zero exit code: 2`가 발생한다. 

Publish directory
* 빌드가 완료된 후 생성되는 폴더 이름
Functions directory
*  서버리스 함수를 개발하고 호스팅하기 위한 기능을 제공하는 디렉토리. 별도로 활용하게 된다면 추가로 포스팅해보겠다! 우선 없으니 빈칸.

![](https://velog.velcdn.com/images/somda/post/f3dac98a-f948-4929-9e92-55df26b32660/image.png)
Environment variables
* 만약 사이트에서 사용한 키 값 등이 있다면 이곳에 넣어주면 된다!
해당 프로젝트에서는 github api를 활용한 게 있어, 해당 key값을 넣어줬다.

모두 입력했다면
하단
`Deploy` 버튼을 눌러보자!

![](https://velog.velcdn.com/images/somda/post/bcc21387-d6f3-449a-8f92-3fdcf1467abc/image.png)
Production deploys에서 현황을 확인할 수 있다.
![](https://velog.velcdn.com/images/somda/post/46fe2501-eb7e-492d-a421-cf00229a1b9c/image.png)
열심히 돌아가는 중...
![](https://velog.velcdn.com/images/somda/post/8548ee07-5b61-46b4-a8fd-9c0dd50cf6d4/image.png)
완료가 되면, overview에서 화면 미리보기에 정상적으로 화면이 뜬다!

### 도메인 수정
Domain settings를 누르면
![](https://velog.velcdn.com/images/somda/post/6a93350a-7f63-424a-864a-363003e88972/image.png)
이렇게 도메인을 수정할 수 있다.
참고로 http도 https로 알아서 잘 변경되어있다.

만약 뒤에 netlify.app도 붙지 않는 도메인을 등록하고 싶다면, 도메인을 구입 후 Add a domain으로 변경할 수 있다.

그리고 변경한 링크를 클릭하면
![](https://velog.velcdn.com/images/somda/post/592f91f3-f085-46aa-99a7-d813e1904abd/image.png)
사이트에서 접속할 수 있다 :)