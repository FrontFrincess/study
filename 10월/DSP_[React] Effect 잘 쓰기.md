# [React] Effect '잘' 쓰기

본 포스팅은 React 공식 사이트의 [effect로 동기화 하기](https://ko.react.dev/learn/synchronizing-with-effects)와
[이펙트가 필요하지 않은 경우](https://ko.react.dev/learn/you-might-not-need-an-effect) 파트를 기반으로 일부 재구성 및 작성되었습니다.

## Effect란
 effect란 렌더링 자체에 의해 발생하는 부수 효과를 특정하는 것으로, 특정 이벤트가 아닌 렌더링에 의해 직접 발생한다. `useEffect`는 함수 컴포넌트에서 effect 관련 작업을 수행할 때 사용되는 Hook 중 하나이다.
 다시 말해, effect는 주로 React 코드를 벗어난 특정 외부 시스템(브라우저 API, 네트워크 등)과 동기화하기 위해 사용된다.

### effect 사용법
1. effect를 선언한다.
2. effect의 의존성을 지정한다.
3. 필요시 clean up 함수를 호출한다.

여기서 3번의 경우 종종 마주한 문제로부터 기인한다.

`useEffect` 안에서 `console.log()`를 사용했을 때 분명 컴포넌트 마운팅은 한 번만 된 것 같은데 두개가 찍히는 경우가 있었을 것이다.

공식 사이트의 코드를 보자.

```js
// App.js
import { useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
  }, []);
  return <h1>채팅에 오신걸 환영합니다!</h1>;
}

//Chat.js
export function createConnection() {
  // 실제 구현은 정말로 채팅 서버에 연결하는 것이 되어야 합니다.
  return {
    connect() {
      console.log('✅ 연결 중...');
    },
    disconnect() {
      console.log('❌ 연결이 끊겼습니다.');
    }
  };
}
```

실행해보면
![](https://velog.velcdn.com/images/somda/post/fd453d0a-d8ee-4b59-ab65-d3d39ebcbeb1/image.png)
이렇게 두 번 실행되는 걸 볼 수 있다.
사실 이 문제는 개발자 모드에서만 발생하는 것으로 react Strict Mode를 없애면 콘솔이 두 번 찍히는 문제는 해결되겠지만 근본적으로 봐보자.

Strict Mode는 이런 버그를 잡기 위해 개발 중에 컴포넌트를 명시적으로 다시 마운트한다. 해당 페이지에서 다른 페이지에 갔다가, 다시 해당 페이지로 돌아온 것과 같은 상황을 의도적으로 만든 것이다.

결국 첫번째 연결은 끊기지 않았고, 한 번 쌓이게 된 것이다. 이 상황에서 clean up 함수를 작성하지 않으면 사용자가 페이지를 왔다갔다 할 수록 이 실행은 쌓이게 된다.

그래서 이러한 문제를 해결하기 위해 clean up함수를 호출해야 한다.

```js
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []);
```

이렇게 return 문을 사용해 cleanup 함수를 호출할 수 있다.

![](https://velog.velcdn.com/images/somda/post/d4f08163-d770-43a8-9540-ceb0349617c1/image.png)

그러면 이렇게 연결이 됐다가 끊어졌다 다시 연결되는 것을 확인할 수 있다.
(물론 실제 배포 환경에서는 최초 마운트 시, 연결 중 한 번만 실행될 것이다.)

이런 경우 외에도 useEffect 안에서 addEventListner를 사용했던 경우 remove를 사용하고, 애니메이션을 설정해줬다면 초기화 하는 과정도 추가로 작성하는 것을 권장한다.


### effect가 필요하지 않은 경우
[공식 사이트](https://ko.react.dev/learn/you-might-not-need-an-effect)에 보다 자세한 상황 별 예시들이 더 많이 나와있으며, 조금 더 상위 개념이라 생각되는 케이스를 일부 정리했다.

**1. 애플리케이션 초기화**
사용자의 브라우저를 확인하는 경우 등 일부 로직은 app이 시작되었을 때 한 번만 실행되면 된다. 이런 경우는 컴포넌트 외부에 로직을 작성하면 된다.

```js
if (typeof window !== 'undefined') { // 브라우저에서 실행 중인지 확인합니다.
  checkAuthToken();
  loadDataFromLocalStorage();
}

function App() {
  // ...
}
```

**2. 일부 data fetch**
`useEffect` 내부에서 fetch를 호출하는 것은 아주 흔하다.
하지만 이는 컴포넌트가 언마운트되고 다시 마운트되면 데이터를 다시 가져와야 하기 때문에 캐싱 등을 사용하는 편이 더 낫다.

또한 물건 구매와 같은 post 요청이 있다고 가정해보자. 해당 요청을 effect 안에서 하게 되면 컴포넌트가 다시 마운트 되는 경우 등 두 번 이상 호출 되는 경우를 사용자가 파악하기 쉽지 않으며 UX에 치명적인 영향을 줄 수 있다.
이 경우는 구매 버튼 클릭 등 이벤트에 기반하여 동작하게 하는 편이 낫다.

**3. 다른 상태에 기반하여 변하는 경우**
state에 저장한 값을 바탕으로 계산이 가능하다면 굳이 effect로 계산할 필요가 없이 렌더링 중에 계산하도록 하면 된다.
이건 props로 받은 경우에도 마찬가지이다.
```js
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ 렌더링 중에 계산됨
  const fullName = firstName + ' ' + lastName;
  // ...
}
```