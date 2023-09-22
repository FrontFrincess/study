# react-device-detect를 사용 기기별 화면을 만들어보자!

![post-thumbnail](https://velog.velcdn.com/images/yjohbjects/post/7259d9fb-6fd3-4929-aa40-5035feb5eda2/image.png)

# react-device-detect

## 사용자 사용 환경 추적 라이브러리
React 라이브러리 중 `react-device-detect`라는 말그대로 사용자의 사용환경을 추적할 수 있어, 보다 기기별 화면을 효과적으로 보여줄 수 있는 라이브러리가 있다!

이 라이브러리는 크롬, 사파리, 익스플로러와 같이 특정 브라우저에서 다른 화면을 보여주려고 할 때 효과적이지만, 반응형 화면을 구성하고싶을 때에는 추천하지 않는다!
반응형 화면은 `CSS @media 쿼리`나 `matchMedia`, `react-responsive`, `@react-hook/media-query`를 사용해야 더 효과적이다.


## 🛠️기능
여기서 추적되는 기기는 생각보다 다양한데,,🤔

모바일, 태블릿, 데스크탑, 스마트 티비, 웨어러블 디바이스 등 `기기`도 추적이 가능하지만
안드로이드, IOS, 크롬, 파이어폭스, 사파리 등 `웹 브라우저` 또한 추적이 가능하다!
버전 추적도 가능한데, 윈도우 7, 안드로이드 6, 크롬 65, 인터넷 익스플로러 9와 같이.. 모든 버전 추적이 가능한건아니지만, 큰 변화가 생긴(?) `버전`들이 추적이 가능한 것으로 보인다!

보다 자세한건 GitHub에 나와있다!
> https://www.npmjs.com/package/react-device-detect
> https://github.com/duskload/react-device-detect

### selectors와  views의 차이
위에 언급된 기능들은 `selecctors`와 `views`로 나뉘는데,
`selectors`는 true를 반환하고, `views`는 컴포넌트를 반환하여 적절히 사용하면 된다!

**`selectors` 예시**: `isMobile`, `isBrowser`
**`views` 예시**: `BrowserView`, `MobileView`

(모바일 뷰가 있다고해서 모바일 설렉터가 있는 것은 아니니까 유의하자)

# 사용 방법
사용방법은 굉장히 간단하다!


## 설치
```bash
npm install react-device-detect
yarn add react-device-detect
```

## import
우선, 사용할 selectors와 views를 가져온다

``` javascript
import {
  BrowserView,
  MobileView,
  isBrowser,
  isMobile,
  isSafari,
  isChrome,
} from "react-device-detect";
```

## 사용 예시
``` javascript
function deviceDetect() {
  return (
    <div>
      {/* views 사용 방법 */}
      <BrowserView>
        <h2>브라우저 화면</h2>
      </BrowserView>
      <MobileView>
        <h2>핸드폰 화면</h2>
      </MobileView>
    
      {/* selectors 사용 방법 */}
      {isBrowser ? <h2>브라우저</h2> : null}
      {isMobile ? <h2>모바일</h2> : null}
      {isSafari ? <h2>모바일</h2> : null}
      {isChrome ? <h2>모바일</h2> : null}
    </div>
  );
}
```

> 제가 뭘 만들었는지 궁금하신가요?ㅎㅋ
> https://princess-study.netlify.app/에서 확인해주세옹?


# 마무리
사용 방법이 생각보다 간단했기 때문에,, 오히려 앞부분 설명이 장황하다고 느껴졌던 포스팅..

앞서 말했듯이 제공되는 추적 기능이 생각보다 범주가 넓어서 알아두면 유용할 라이브러리라고 생각한다.
다만 반응형 화면 구성 목적이 아닌, `인터넷 익스플로러는 지원되지 않습니다. 크롬으로 접속해주세요.`와 같은 환경적 제한 목적으로 사용하는 데에 유용할 것이다!