# 'Responsively'로 기기별 웹페이지 한번에 모아보기!

![post-thumbnail](https://velog.velcdn.com/images/yjohbjects/post/6e006c58-0bed-4103-8b03-032e6fa3a994/image.png)

반응형 웹페이지를 만들 때 핸드폰, 태블릿, 컴퓨터에서 보여지는 화면을 어떻게 확인할까?
개발자 도구를 켜서 일일히 디멘션을 바꾼다..?
기기별로 직접 접속해서 화면을 확인한다..?
너무 피곤해!!

이 글을 보고 나서는 'Responsively'를 통해서 한번에 기기별 화면들을 모아볼 수 있다!
또한 하나의 기기에 준 이벤트는 다른 기기 화면에도 동시 적용이 되기 때문에, 이렇게 편리할 수가 없다. 잠깐... 게다가 무료... ??
그렇다면 사용법을 알아보자~!

### 1. 설치
우선 'Responsively'를 설치해아한다
아래 링크를 통해서 프로그램 설치를 하고~!
> https://responsively.app/download

설치된 후 프로그램을 실행하면 이런 화면이 뜬다!
![](https://velog.velcdn.com/images/yjohbjects/post/f5b06be4-26a0-4ee9-810d-5d97bd2c45f0/image.png)

기본적으로 보여지는 기기는 아이폰 12 Pro, 아이패드, 맥북 프로로 구글 랜딩 페이지가 보여진다.
하지만 간단한 설정을 통해서 애플 기기 뿐만 아니라, 삼성과 마이크로소프트, 블랙베리 등의 기기까지도 설정이 가능하다.


### 2. 웹페이지 접속
![](https://velog.velcdn.com/images/yjohbjects/post/33cf45a7-33eb-4af6-9376-46588b90e220/image.png)
원하는 웹페이지에 접속하는 것은 정말 간단하다!
웹페이지에 접속하는 방법과 동일하게, 기존 웹 주소를 클릭해서 수정한뒤 접속하면 끝.

### 3. 기능
'Responsively'는 단순히 페이지만을 보여주는 것이 아니라, 실제 기기의 환경을 테스트해볼 수 있는 기능들이 많이 있다!
하나씩 소개해보겠다!!

## 기기 화면들을 위한 기능

![](https://velog.velcdn.com/images/yjohbjects/post/98903a94-7fb8-4952-8925-1f08bbb76e77/image.png)

1. `Delete Storage`: 스토리지를 한번에 삭제
2. `Delete Cookie`: 쿠키를 한번에 삭제
3. `Clear Cache`: 캐시를 한번에 삭제
4. `Homepage`: 현재 페이지를 홈페이지로 설정
5. `Add Bookmark`: 북마크 추가


![](https://velog.velcdn.com/images/yjohbjects/post/f8351040-c267-417b-8c55-c9574c799761/image.png)

1. `Rotate Devices`: 기기 회전 (가로 <-> 세로)
2. `Inspect Elements`: 마우스오버해서 요소 검사
3. `Screenshot All Webviews`: 현재 보여지는 기기 화면들을 모두 캡쳐
4. `Device Theme Color Toggle`: 기기의 맞춤 테마 설정
5. `View Shortcuts`: 기기별 단축키 소개
6. `Manage Suites`: 기기를 추가할 수 있고, `Default`설정 시 초기의 아이폰 12 Pro, 아이패드, 맥북 프로 기기설정으로 돌아온다.

  \* 여기서 Suite이라고 하는 이유는 확인하고 싶은 기기들을 하나의 Suite으로 묶어서 볼 수 있기 때문이다. 예를들어, 아래를 보면 나는 삼성 기기들을 묶어서 하나의 Suite을 만들었다.
  ![](https://velog.velcdn.com/images/yjohbjects/post/c49557bc-d8ae-4814-8c10-cad9039b81e3/image.png)
  \* Suite을 만들 때, 원하는 기기가 없어서 걱정이라면 custom device도 추가할 수 있으니 걱정 싸악~~!!
  ![](https://velog.velcdn.com/images/yjohbjects/post/32d3d12a-e95e-4b47-9f36-166111956e06/image.png)

  7. `Menu`: 그 외 각종 기능을 활용 가능하다
    ![](https://velog.velcdn.com/images/yjohbjects/post/01914e45-01d1-4cbf-a21f-bcaddd3b95b0/image.png)

	
     - `Zoom`: 화면 확대
     - `Preview Layout`: 화면 레이아웃 변경 (1횡 <-> 모아보기)
     - `UI Theme`: 'Responsively' 테마 변경 (다크모드 <-> 라이트모드)
     - `Dock Devtools`: 소스코드를 볼때 화면내 Dock으로 볼지 여부
     - `Allow Insecure SSL`: 사용하고 있는 인증서를 항상 신뢰
     - `Clear History`: 'Responsively' 내 기록 삭제
     - `Bookmarks`: 저장된 북마크 조회
     - `Settings`: 그 외 'Responsively' 세팅
     

## 각 화면을 위한 기능
![](https://velog.velcdn.com/images/yjohbjects/post/1d1e759c-7063-40e5-9d73-c19454e3310c/image.png)
개발을 하다보면 한 화면을 위한 기능 테스트도 필요하지 않은가?
'Responsively'는 준비되어있다!

1. `Refresh This View`: 해당 기기 화면 새로고침
2. `Quick Screenshot`: 해당 기기에 보여지는 화면 캡쳐
3. `Full Page Screenshot`: 해당 기기에 보여지는 페이지 전체 캡쳐
4. `Disable Event Mirroring`: 해당 기기만 이벤트 미러링 중지
5. `Open Devtools`: 해당 기기의 개발자 도구 열기
6. `Rotate Device`: 해당 기기 방향 전환 (가로 <=> 세로)

# 마무리
이렇게 해서 'Responsively'의 기능들을 둘러보았다.
웹 주소로 접속해야해서, 개발 도중에는 시시때때로 화면을 확인하기 불편함은 있겠지만
`ngrok`와 같은 로컬 컴퓨터의 개발 환경을 인터넷으로 공유해주는 툴을 활용한다면 시시때때로 확인도 가능하다는 사실.. (다음엔 `ngrok` 사용법을 가져와볼까..?)
아무튼 반응형 웹페이지 개발 단계의 중요 phase마다 모니터링 해보기 좋은 툴인 것은 확실하다!
이사앙~!!


> source
> https://responsively.app/