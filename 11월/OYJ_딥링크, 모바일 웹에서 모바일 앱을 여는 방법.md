# 딥링크, 모바일 웹에서 모바일 앱을 여는 방법

![post-thumbnail](https://velog.velcdn.com/images/yjohbjects/post/90c5b8b0-f19c-4f00-b275-febd683b69d7/image.png)

## 딥링크
특정 주소로 앱을 실행시키는 기능이다.
기기와 용도, 문법에 따라 다른 스킴유형을 쓰는데, 오늘은 모바일 웹에서 모바일 앱을 여는 URL 스킴 유형의 딥링크를 알아보겠다!

### URL 스킴 딥링크
iOS와 Android에서 동일하게 사용되는 딥링크 방식
> **문법**: Scheme://Path

- `Scheme`: 앱에서 설정한 고유한 값
- `Path`: 실행할 앱에서 이동하고싶은 특정 화면
_주의 `Path`가 없어도 일반적으로 앱을 여는 데에 동작은 하지만, 특정 앱들은 `Path`가 있어야만 앱이 열리게 구현해 놓은 경우도 있음_

예시)
![](https://velog.velcdn.com/images/yjohbjects/post/45f98dac-141b-43a7-80ed-f6b47334c136/image.png)


### 사용 방법
URL이동으로 간단하게 특정앱으로 이동 가능! 정말 간단하다.
구글에서 찾아보면 URL Scheme을 정리해놓은 문서들도 있지만, 가장 좋은 것은 공식 문서!

예를들어 카카오 지도 앱을 열고싶다고 가정하자.
카카오 지도 앱의 URL Scheme 공식문서를 찾는다.
(https://apis.map.kakao.com/ios/guide/#urlscheme)

공식문서를 통해 `Scheme`은 `kakaomap://`이라는 것을 쉽게 확인할 수 있고,
이제 원하는 `Path`를 찾아본다!

카카오맵 공식문서는 친절하게도, 기능별로 `Path`를 정리해두었다.
그렇다면, 그 중에서도 특정 장소에 마커가 표시된 앱 화면을 띄워보겠다.

```typescript
<a href="kakaomap://place?id=7813422">카카오에서 장소로 이동하기로 설정해 놓은 장소</a>
```

그럼 완성!
![](https://velog.velcdn.com/images/yjohbjects/post/6b66eb34-4cdc-4cba-b28a-43fd599c88a8/image.gif)


## 단점
단점이라 쓰고, 유의사항이라고 읽는다!
딥링크를 사용할때 꼭 유의해야하는 점들이 있다.

### 유연성
핸드폰에 앱이 설치되어있지 않을 경우와 같은 예외케이스들은 직접 설정해주어야한다.
위에 예시로 들었던 카카오맵의 경우는 예외처리하기 섹션을 두어 친절하게도 설명을 해주었지만, 
앱이 설치되지 않은 디바이스의 경우 웹페이지로 이동한다던지, 앱스토어로 이동하는 동작을 주려면 개인적으로 처리해야한다.

### 보안
Scheme은 앱에서 설정할 수 있는 고유한 값이지만, 다른 앱과 중복될 수 있다.
즉 URL 스킴이 같은 앱이 여러개 있을 수도 있다는 얘기. 드문 경우이겠지만, 이미 갤럭시에서는 `market://`스킴으로 여러 어플이 확인된다.

![](https://velog.velcdn.com/images/yjohbjects/post/3170ae45-778e-40c2-b55b-c8acf55f7409/image.png)
이런 경우, 원하는 앱으로의 이동이 실패할 수 있으니, 꼭 여러 환경에서 확인해볼 필요가 있다.
또는 다른 유형의 딥링크를 사용해보는 것도 방법이다. Apple에서는 공식적으로 Universal Link를 사용하는 것을 추천한다고 한다. (하지만 URL Scheme이 가장 범용적으로 사용되기 때문에 오늘은 URL Scheme만 가져왔다)

> 참고자료
> https://velog.io/@tosspayments/%EB%94%A5%EB%A7%81%ED%81%AC-%EC%8B%A4%EC%A0%84%EC%97%90%EC%84%9C-%EC%9E%98-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95