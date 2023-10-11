원문 : [[React] 따봉 React.lazy야 Lighthouse 성능 개선을 부탁해!!](https://hi-claire.tistory.com/69)

[##_Image|kage@oGtco/btsx2sKb9Ug/1bZ72uOKpk1F5iQsufXgmk/img.jpg|CDM|1.3|{"originWidth":2400,"originHeight":1350,"style":"alignCenter","width":724,"height":407,"filename":"react.lazy배너.jpg"}_##]

## **0\. 서론**

바야흐로 HourGoods...

Lighthose 점수를 너무 높이고 싶었던 규투리...

폭풍검색을 하여 React.lazy()를 사용해서 **79 -> 98** 이라는 킹성비 성능 개선을 이루어냈으나!!!!

대충 코드스플리팅 때문에 성능 점수가 확 올랐구나 정도만 알고, 정확히 어떤 메커니즘으로 동작이 되는지 알지 못했기에 조금 늦었지만 제대로 다시 공부해보고자 한다.

## **1\. 코드 분할(Code Splitting)이 필요한 이유**

코드 분할을 하면 lighthouse 성능 점수가 확 뛰는 이유가 있다.

바로 **사용하지 않는 자바스크립트를 줄일 수 있기 때문**이다.

**프로젝트가 크고 복잡해질수록 번들도 커진다.**

이때, 번들이 거대해지는 것을 방지하기 위해 가장 좋은 해결방법은 **코드를 나누는 것**이다.

모던 웹이 발전할수록 점점 DOM을 다루는 정도가 정교해지고, 그에 따라 자바스크립트 코드또한 방대해지고 무거워졌다.

즉, 자바스크립트 엔진이 해석해야 하는 자바스크립트 코드의 양이 많아진 것이다.

이때 코드 분할을 하면 **런타임에 여러 번들을 동적으로 만들고 불러올 수 있게** 된다.

코드 분할은 앱을 _**'lazy'**_ 하게 로딩하도록 도와준다.

초기 로딩에 필요한 비용을 줄여주기 때문에 **사용자는 빠른 렌더링을 경험**할 수 있고, **개발자는 앱의 코드 양을 줄이지 않고도 성능 향상**을 이룰 수 있게 된다. ~(개이득)~

## **2\. 그렇담 코드 분할은 어떻게 도입하느냐?**

가장 간단한 방법이 동적 `import()` 문법을 사용하는 것이다.

```
// 이랬던 것을 (static import)
import { add } from './math';
console.log(add(2, 4));

// 이렇게 바꿨습니다 (dynamic import)
import('./math').then(math => {
	console.log(add(2, 4));
});
```

코드 파일의 최상위에서 `import`문으로 라이브러리 및 파일을 불러오는 것을 static import라고 한다.

**static import**의 경우 `import`문이 실행될 때 해당 **모듈이 즉시 로드**되며, 이로 인해 모든 `import`문이 실행되기 전까지 다음 코드가 실행되지 않는다. 그렇기 때문에 만약 import되는 모듈이 크면 로딩 시간이 길어질 수밖에 없다.

반면, **dynamic import**의 경우 **모듈을 비동기적으로 로드**하기 때문에, 함수가 호출될 때까지 모듈이 로드되지 않는다.

필요한 시점에 필요한 모듈만 로드하여 초기 번들 크기를 줄여서 초기 로딩 시간을 최적화할 수 있는 것이다. (성능 최적화는 덤!)

## **3\. React.lazy()란?**

`React.lazy()`는 React의 **동적 코드 분할(Dynamic Code Splitting)**을 지원하기 위한 함수이다.

React는 SPA이기 때문에 한 번에 사용하지 않는 컴포넌트까지 불러온다. (즉, 불러올게 많으면 초기 로딩 시간도 늘어난다.)

`React.lazy()`를 사용하면 **컴포넌트를 동적으로 import**할 수 있기 때문에 초기 렌더링 지연 시간을 어느정도 줄일 수 있게 된다.

위의 코드 분할과 마찬가지로 아래와 같이 컴포넌트를 동적으로 import 할 수가 있다.

```
// 이랬던 것을 (static import)
import OtherComponent from './OtherComponent';

// 이렇게 바꿨습니다 (dynamic import)
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

`React.lazy()` 는 `import()` 구문을 반환하는 콜백 함수를 인자로 받는다.

동적으로 불러오는 컴포넌트 파일에는 반드시 지켜줘야하는 두 가지 규칙이 있다.

1.  React 컴포넌트를 포함해야 한다.
2.  default export를 가진 컴포넌트여야 한다.

## **4\. <Suspense />**

`React.lazy()`로 불러온 컴포넌트는 비동기적으로 렌더링되기 때문에 해당 컴포넌트가 로딩될 때가지 기다리는 방법이 필요하다.

이때 사용하는 것이 **Suspense**이다.

```
<Suspense fallback={<div>스피너나 Loading화면을 여기에 넣어준다</div>}>
	<OtherComponent />
</Suspense>
```

**Suspense 컴포넌트는 lazy 컴포넌트를 감싸서 사용**한다.

lazy 컴포넌트는 Suspense 컴포너트 하위에서 렌더링되어야 하며, Suspense는 lazy 컴포넌트가 로드되기 기다리는 동안 **로딩 화면과 같은 예비 콘텐츠를 보여줄 수 있게** 해 준다.

fallback prop으로 컴포넌트가 로드될 때까지 기다리는 동안 스피너를 적용시켜 보여줄 수 있게 하면 된다.

더보기

더보기

현재 Suspense의 경우 SSR을 지원하지 않고 있기 때문에, 서버에서 렌더링하는 경우 React 문서에서 권장하는 [loadable-compoenets](https://loadable-components.com/docs/server-side-rendering/)와 같은 다른 라이브러리를 사용하는 것이 좋다.

## **5\. React Router와 함께 사용해보자**

React 공식문서에 따르면 Router 바로 아래 Suspense를 위치시키고 라우트로 보여줄 컴포넌트들을 `React.lazy()`로 불러올 것을 권장하고 있다.

React Router를 사용하면 각 라우트를 다른 페이지로 생각할 수 있기 때문에 `React.lazy()`를 사용하여 각 페이지 컴포넌트를 동적으로 로드하기 수월해진다.

```
import { lazy, Suspense } from "react";
import { Route, Routes } from "react-router-dom";

const RealTimePage = lazy(() => import("@pages/RealTime"));
const SearchPage = lazy(() => import("@pages/Search"));
const SignupPage = lazy(() => import("@pages/Signup"));

export default function Routers() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<RealTimePage />} />
        <Route path="/search" element={<SearchPage />} />
        <Route path="/signup" element={<SignupPage />} />
      </Routes>
    </Suspense>
  );
}
```

위와 같이 'lazy'하게 로딩할 컴포넌트들을 불러오고,

해당 컴포넌트를 Suspense 컴포넌트로 감싸서 사용해주면 된다.

**결과**

[##_Image|kage@brZzhd/btsx2p7XrYr/gVwy7y4FJrcM8G68kh5kq1/img.png|CDM|1.3|{"originWidth":1340,"originHeight":450,"style":"alignCenter","width":800,"height":269}_##]

\[참조\]

[https://ko.legacy.reactjs.org/docs/code-splitting.html](https://ko.legacy.reactjs.org/docs/code-splitting.html)

[https://web.dev/articles/code-splitting-suspense?hl=ko](https://web.dev/articles/code-splitting-suspense?hl=ko)

[https://wikidocs.net/197644](https://wikidocs.net/197644)