원문 : [[React] 리액트 서버 컴포넌트(RSC)란?](https://hi-claire.tistory.com/70)

[##_Image|[kage@beUPnS/btsy3Ofj86q/Tf5qWUcuOShpysfdf8gtD0/img.png|CDM|1.3|{"originWidth":2400,"originHeight":1350,"style":"alignCenter","width":743,"height":418}](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbeUPnS%2Fbtsy3Ofj86q%2FTf5qWUcuOShpysfdf8gtD0%2Fimg.png)_##]

[요즘IT](https://yozm.wishket.com/magazine/detail/2271/?utm_source=stibee&utm_medium=email&utm_campaign=newsletter_yozm&utm_content=contents)를 읽으며 흥미로운 주제 발견! "React Server Component(RSC)" 라는 것이 있다는 것이다!

최근 리액트 생태계에서 가장 핫한 이슈로 개발자들의 주목을 받고 있다고 해서 우리 프론트공주들과 함께 읽어보면 좋을 것 같아서 해당 주제를 가져 왔다. ~(그리고 너무 내용이 어려워서 후회했다..🥲)~

## **1\. RSC, 서버 컴포넌트가 뭐지..?🤔**

React Server Components는 React 18 버전부터 도입된 개념으로, 말 그대로 **서버에서 동작하는 컴포넌트**를 가리킨다.

**서버 측에서 컴포넌트를 실행하여 미리 렌더링된 마크업을 생성한 후, 클라이언트에게 전달되어 사용자에게 콘텐츠를 보여주는 방식**이다.

#### **CSR vs SSR** 

고전 웹 프론트엔드는 MPA 구조로 SSR 방식을 택했다. 하지만, 웹개발이 더욱 커지고 정교해짐에 따라 프로젝트의 규모가 커졌고, 전체 페이지를 다시 로드하는 시간이 길어짐에 따라 사용자 경험을 향상시키 위해 CSR 방식의 SPA 구조가 모던 웹 패러다임으로 등장한 것이다.

CSR은 서버에서 html을 받고 javascript를 사용하여 웹페이지를 동적으로 렌더링한다. 이때, 기능이 추가될 수록 js번들의 크기가 커짐에 따라 초기 로딩이 느려지고, 사용자는 텅 빈 빈 화면만을 바라보게 된다. 또한, 텅 빈 html 파일을 다운받기 때문에 SEO에 대한 문제점을 갖기도 한다.

[##_Image|kage@eiz2Ws/btszbwD6Vkb/GRxsuCIcIkOyVFVp56THgK/img.png|CDM|1.3|{"originWidth":908,"originHeight":502,"style":"alignCenter","width":661,"height":365,"caption":"CSR","filename":"스크린샷 2023-10-25 오후 4.02.14.png"}_##]

그래서 등장한 것이 SSR이다. 사용자와 상호작용을 하는데에는 여전히 시간이 걸리겠지만, js 파일을 다운받기 전까지 서버로부터 완성된 html을 제공받아 사용자에게 유의미한 콘텐츠를 노출시킬 수 있다는 점이 장점이다. 또한, 미리 렌더링된 html 파일이 넘어오기 때문에 검색 엔진 크롤러가 용이하게 정보를 검색할 수 있다.

[##_Image|kage@cdZCvL/btsy8uNN3uh/N3Mbr4ETSBwLK0uympFNd0/img.png|CDM|1.3|{"originWidth":930,"originHeight":516,"style":"alignCenter","width":658,"height":365,"caption":"SSR","filename":"스크린샷 2023-10-25 오후 4.02.38.png"}_##]

#### **RSC != SSR**

🙋🏻‍♀️ : 으잉? SSR과 같은 것 아닌가요?

👩‍🏫 : 아님다.

서버라는 단어가 둘 다 들어간다는 점 때문에 헷갈릴 수도 있다. 하지만, RSC와 SSR은 서버에서 처리를 한다는 공통점 외에는 각각 해결하고자 하는 목표가 다르고, 결과물 또한 다른 완전히 별개의 개념이다.

[##_Image|kage@5fw4c/btsy8jTjDNH/3Kd7twTrEtZg5xwxxqrnoK/img.png|CDM|1.3|{"originWidth":1128,"originHeight":724,"style":"alignCenter","width":645,"height":414,"caption":"RSC","filename":"스크린샷 2023-10-25 오후 4.04.08.png"}_##]

데이터베이스 쿼리를 최초 요청의 일부로서 수행하기 때문에, RSC는 SSR과 다르게 **완전히 채워진 UI를 사용자에게 바로 전송**할 수 있습니다. SSR을 수행하더라도 모든 컴포넌트는 서버와 클라이언트 양쪽에서 렌더링을 해야하는 반면, 완전히 서버에서 렌더링된 채 넘어오는 RSC는 더욱 강력하다.

#### **서버 컴포넌트 vs 클라이언트 컴포넌트**

리액트 서버 컴포넌트를 사용하기 이전에는 모든 리액트 컴포넌트들은 **클라이언트 컴포넌트**이다.

클라이언트 컴포넌트라니 컴포넌트가 클라이언트에서만 렌더링될 것 같지만, 실제로는 클라이언트와 서버 양측 모두에서 렌더링 된다.

[##_Image|kage@WaPAL/btsy5gbM87H/yCwUaLnnsYUS5JgsGqQYZ0/img.png|CDM|1.3|{"originWidth":1132,"originHeight":458,"style":"alignCenter","width":640,"height":259,"filename":"스크린샷 2023-10-25 오후 4.11.31.png"}_##]

즉, 우리가 잘 알고 있는 **"표준" 리액트 컴포넌트를 클라이언트 컴포넌트**이고, **서버에서만 렌더링 되는 새로운 유형의 컴포넌트가 서버 컴포넌트**인 것이다.

[##_Image|kage@nm7G8/btsy5c78W84/qZBhSfQXo2kBvEJKlbmJoK/img.png|CDM|1.3|{"originWidth":1280,"originHeight":982,"style":"alignCenter","width":761,"height":584}_##]

위 사진에서 볼 수 있듯이, RSC와 RCC는 렌더링되는 위치가 다르기 때문에 각각 할 수 잇는 역할이 명확히 구분된다.

즉, 기존의 리액트 컴포넌트가 수행하지 못했던 다양한 기능들을 서버 컴포넌트를 잘 활용하면 수행할 수 있다.

때문에, RSC와 RCC를 각각 필요한 기능에 따라 적재적소에 배치하여 개발하려는 접근이 필수적이다.

#### **정리하자면.... 🤪**

**리액트 서버 컴포넌트는 서버 사이드 렌더링을 대체하지 않는다!** 🙅‍♀️

**마찬가지로, 리액트 서버 컴포넌트는 클라이언트 컴포넌트를 대체할 수도 없다! 🙅‍♀️**

이 각각의 개념들은 서로를 보완하는 개념이라 생각하면 편하다.

초기 html을 생성하기 위해서는 여전히 서버 사이드 렌더링에 의존해야 한다.

하지만, 리액트 서버 컴포넌트는 특정 컴포넌트가 클라이언트 쪽 자바스크립트 번들에 포함되지 않게, 즉, 서버에서만 실행되도록 할 수 있다.

## ****2\. 그렇다면..  서버 컴포넌트는 어떻게 동작하는가?****

~솔직히 완벽히 이해 못 했다..~

아래의 그림과 같이 RSC와 RCC가 적절히 혼합된 컴포넌트 트리가 존재한다고 가정하자. 사용자는 해당 페이지를 띄우기 위해 서버로 요청을 보낸다. 그러면 서버는 이때부터 컴포넌트 트리를 root부터 실행하며 **직렬화된 JSON형태로 재구성**하기 시작한다.

[##_Image|kage@lAo2P/btszbh1m8sr/a3xc7J7jj3V8iWyvL0ubbk/img.png|CDM|1.3|{"originWidth":990,"originHeight":765,"style":"alignCenter","width":525,"height":406}_##]

> **💡 직렬화(**Serialization**)란?**  
> 데이터 스토리지 문맥에서 데이터 구조나 오브젝트 상태를 동일하거나 **다른 컴퓨터 환경에 저장**하고 **나중에 재구성할 수 있는 포맷으로 변환**하는 과정

우리가 흔히 사용하는 JSON.stringify 함수가 바로 직렬화를 수행하는 함수이며, JSON.parse가 역직렬화를 수행하는 함수다.

이때, 서버는 모든 객체를 직렬화할 수 없다.

**함수(Function), 날짜(Date) 등의 데이터는 직렬화가 불가능한 데이터**로 취급되므로 서버 컴포넌트에서는 해당 부분을 해석하지 않고 건너 뛰게 된다.

대표적으로 서버 컴포넌트에서 사용할 수 없는 코드들은 다음과 같다.

-   useState, useEffect, useReducer 등의 State Hooks
-   DOM, 브라우저 API를 사용한 코드 (이벤트 리스너)
-   RCC

건너 뛴 데이터의 경우 직접 서버가 해석하는 것이 아닌, "이곳은 RCC가 렌더링되는 위치입니다" 와 같은 placeholder를 배치해주며 직렬화 작업을 진행한다.

[##_Image|kage@I2FgF/btsy8ljotjD/5AT2AYrb9wlHAikJuZzDW0/img.png|CDM|1.3|{"originWidth":1280,"originHeight":1082,"style":"alignCenter","width":595,"height":503}_##]

이제 이렇게 도출된 결과물을 Stream 형태로 클라이언트가 전달받게 되고, 함께 다운로드한 JS 번들을 참조하여 module reference 타입이 등장할 때마다 RCC를 렌더링해서 빈 공간을 채워놓는다. 마지막으로 DOM에 반영하면 실제 화면 스크린이 보여지게 되는 것이다.

[##_Image|kage@NmMYp/btsy8uf4mwx/7Mik1LNj42pEipYkng9h7k/img.png|CDM|1.3|{"originWidth":1280,"originHeight":1114,"style":"alignCenter","width":628,"height":547}_##]

결론적으로 RSC 스트림은 아래와 같이 반환된다.

```
M1:{"id":"./src/ClientComponent.client.js","chunks":["client1"],"name":""}
S2:"react.suspense"
J0:["$","@1",null,{"children":[["$","span",null,{"children":"Hello from server land"}],["$","$2",null,{"fallback":"Loading tweets...","children":"@3"}]]}]
M4:{"id":"./src/Tweet.client.js","chunks":["client8"],"name":""}
J3:["$","ul",null,{"children":[["$","li",null,{"children":["$","@4",null,{"tweet":{...}}}]}],["$","li",null,{"children":["$","@4",null,{"tweet":{...}}}]}]]}]
```

이렇게 RSC는 모든 데이터를 기다리는 것이 아니라,

한 행이 수행 완료된다면 해당 행을 즉각적으로 반영하여 작업하고, 아직 그릴 수 없는 부분은 체크만 해두고 넘어간다.

그리고 이것은 raw html이 아니라 위의 코드와 같은 스트림 형태의 JSON으로 이루어진다.

## **3\. 그래서 RSC는 왜 쓰는 건데🤯**

기존 SSR 방식에는 몇가지 단점이 존재했다.

**1\. 데이터 패칭의 한계**

SSR 방식에서는 페이지가 아닌 컴포넌트를 정적으로 export할 수 없다. 실질적으로 리액트 개발자가 조작하는 코드는 컴포넌트 단위로 구성되는데, 컴포넌트를 마음대로 다룰 수 없는 점은 불편한 요소이다.

Next.js를 다뤄봤다면, pages 하위에 있는 컴포넌트가 아닌 이상 SSR 관련 함수들(ex. getServerSideProps)를 사용할 수 없다는 것을 알고 있을 것이다. 그렇기 때문에 최상위가 되는 **pages 컴포넌트에서 서버단으로부터 fetch해온 데이터를 아래로 prop** 해줘야 하거나, **하위 컴포넌트에서 CSR 방식으로 데이터를 가져오는 방법**을 택해야 했다.

해당 방식들은 부모-자식 컴포넌트 간의 의존성을 높여 유지보수를 힘들게 하며, **불필요한 정보까지 over-fetching** 하게 된다.

또한, 부모 컴포넌트가 렌더링이 완료되어야 자식 컴포넌트 아래로 데이터를 prop 시킬 수 있기 때문에, 자식 컴포넌트의 렌더링이 지연되는 **데이터패칭 waterfall 현상**이 발생한다.

**2\. 라이브러리 번들 사이즈의 문제**

리액트는 기본적으로 개발에 필요한 다양한 외부 라이브러리들을 적극 사용하며 프로젝트를 진행한다. 하지만, 프로젝트가 커짐에 따라, 라이브러리 사용이 많아짐에 따라 필연적으로 코드와 번들 사이즈가 증가하게 된다.

SSR의 경우, UI를 렌더링하는 데 필요하지 않는 데이터 처리 과정에서 사용되는 모듈까지 함께 번들링이 진행되니 큰 규모의 프로젝트인 경우 브라우저가 받아와야 하는 파일의 용량이 매우 높아진다.

이러한 한계를 극복하고자 RSC 개념이 도입됐다.

---

#### **RSC의 장점⚡️**

**1\. 환경별 리소스 접근**

서버 컴포넌트는 말 그대로 서버에서 동작하는 컴포넌트이다.

page 단에서만 서버 데이터에 접근할 수 있는 Next.js와는 다르게, RSC를 사용하게 된다면 **일반적인 단위의 컴포넌트에서도 백엔드 리소스에 접근이 가능**하다.

```
import fs from 'fs';
import db from 'db';

// 서버 컴포넌트 예제
const ServerComponent = () => {
  const file = fs.readFileSync('../'); // 파일 시스템 접근 가능
  const data = db.post.load();         // 데이터베이스 접근 가능

  return (
    <div></div>
  );
}
```

SSR 기반의 Next.js의 경우 컴포넌트에서 **로컬스토리지 혹은 쿠키와 같은 브라우저에 접근하는 데이터를 자유롭게 사용할 수 없다**. 해당 컴포넌트를 dynamic import를 사용하여 서버에서 렌더링 되지 않도록 비활성화를 해줘야 한다.

서버 컴포넌트의 경우, **브라우저에 접근하는 데이터가 필요한 컴포넌트는** **클라이언트 컴포넌트로 정의하여 사용**을 하면 된다!

리액트 서버 컴포넌트 패러다임에서는 기본적으로 모든 컴포넌트가 서버 컴포넌트라고 가정한다.

```
'use client';	// 클라이언트 컴포넌트라고 명시

import React from 'react'

function Counter() {
	const [count, setCount] = React.useState(0);	// state Hooks 사용 가능
    const userInfo = localStorage.getItem('user');	// 브라우저에 접근 가능
    
    return (
    	<button onClick={() => setCount(count + 1)}>
        	Current value: {count}
        </button>
    )
}
```

위의 코드와 같이 상단에 `'use client'` 를 명시함으로써, 해당 컴포넌트는 클라이언트에서 다시 렌더링할 수 있도록 JS 번들에 포함해야한다는 것을 알려준다.

**2\. 제로 번들 컴포넌트**

프로젝트가 커짐에 따라 같이 번들 사이즈 또한 커진다.

하지만 서버 컴포넌트는 **해당 컴포넌트의 코드 및 번들을 서버에서 다운로드**를 하기 때문에, **번들이 브라우저에 전송되지 않는다**.

위에서 설명한 직렬화 가능한 상태로 클라이언트에게 JSON 데이터를 전달하여 렌더링하는 방식이기 때문이다.

거대한 번들이 포함되지 않는만큼 **사용자에게 기존보다 훨씬 빠르게 컴포넌트를 제공**할 수 있다.

## **4\. 마치며.....🥲**

솔직히 아직은 나에게 서버 컴포넌트라는 개념이 너무너무너무 어려웠기 때문에, 아직 완벽하게 이해를 하지는 못했다.

하지만, 현재 React 18 뿐만 아니라, 최근에 출시된 Next.js 13 버전까지 해당 개념을 적용시키고 있기 때문에, 트렌드를 공부해보고자 어렵지만 끝까지 포스팅을 마친다. 데이터와 UI를 서버와 클라이언트로 잘 분리해서 사용한다면... 서버 컴포넌트라는 놈이 아주 유용하게 쓰일 것 같기 때문에, 나 또한 현업에 들어가게 된다면 꼭 적용해보고 싶은 신기술이다.

많이 틀린 부분이 있을 것이라 예상이 되기에, 틀린 부분이 있다면 댓글로 지적해주시면 달게 받겠다!!! 🙏

\[참고\]

[https://yozm.wishket.com/magazine/detail/2271/?utm\_source=stibee&utm\_medium=email&utm\_campaign=newsletter\_yozm&utm\_content=contents](https://yozm.wishket.com/magazine/detail/2271/?utm_source=stibee&utm_medium=email&utm_campaign=newsletter_yozm&utm_content=contents)

[https://youtu.be/TQQPAU21ZUw?si=Rs1KZIiIhkZUftWo](https://youtu.be/TQQPAU21ZUw?si=Rs1KZIiIhkZUftWo)

[https://velog.io/@2ast/React-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8React-Server-Component%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0](https://velog.io/@2ast/React-%EC%84%9C%EB%B2%84-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8React-Server-Component%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B3%A0%EC%B0%B0)

[https://yceffort.kr/2022/01/how-react-server-components-work#rsc%EC%97%90%EC%84%9C-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C](https://yceffort.kr/2022/01/how-react-server-components-work#rsc%EC%97%90%EC%84%9C-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

[https://yiyb-blog.vercel.app/posts/look-around-server-components](https://yiyb-blog.vercel.app/posts/look-around-server-components)

[https://haesoo9410.tistory.com/404](https://haesoo9410.tistory.com/404)
