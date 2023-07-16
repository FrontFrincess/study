 NEXT토스에 지원해서 과제 전형을 치루는데, 채점을 하기 위한 테스트 코드가 작성되어 있었다. 테스트 코드 말로만 들어봤지 실제로 작성된 코드를 본 것은 처음이라 신기했고, 이참에 간단하게라도 직접 짜보자! 싶더라. (시험은 시원하게 말아 먹었다. 😥)

 프론트엔드의 테스트 도구로는 Jest, Jasmine, Mocha, Enzyme, React Testing Library 등이 있는데 우선 CRA를 통해 바로 접할 수 있는 React Testing Library(with JEST)를 사용해볼까 한다.

### 1. 테스트할 기능 준비
 먼저 테스하기 위해 App.js 파일에 간단한 기능을 구현했다.
 ![](https://velog.velcdn.com/images/somda/post/5116b567-0fc9-457c-a7cb-355deef9694b/image.gif)
더하기 버튼을 누르면 1씩 증가하고, 빼기 버튼을 누르면 1씩 감소하는 페이지다. 

```js
import { useState } from "react";
import "./App.css";

function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <h3>{count} </h3>
      <button
        type="button"
        onClick={() => {
          setCount(count + 1);
        }}
      >
        더하기
      </button>
      <button
        type="button"
        onClick={() => {
          setCount(count - 1);
        }}
      >
        빼기
      </button>
    </div>
  );
}

export default App;
```

### 2. 테스트 코드 살펴보기
 테스트 코드를 작성하기 전에 cra를 하면 기본적으로 생기는 App.test.js 파일을 살펴보자.

```js
import React from 'react';
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```
![](https://velog.velcdn.com/images/somda/post/2381ec3e-2b5e-4f07-9f6a-76263f0cc9a5/image.png)
리액트 로고가 뺑글뺑글 돌아가는 기본 화면 하단에 있는 `Learn React`가 잘 보이는지 확인하는 코드다.

`render(<App />);`은 App 컴포넌트를 렌더링 시키고,`expect(linkElement).toBeInTheDocument();`부분은 linkElement(lean react라는 텍스트로 표시된 엘리먼트)가 선택된 엘리먼트가 화면에 표시된 DOM에 존재하는지를 검증한다.
`const linkElement = screen.getByText(/learn react/i);` 이 부분에서는 learn react라는 텍스트를 정규표현식으로 작성했다. i 플래그를 통해 대소문자를 가리지 않고 검증한다.


### 3. 그럼 이제 직접 써보자!
 [testing-library](https://testing-library.com/docs/react-testing-library/api#container)와 [jest](https://jestjs.io/docs/api) 홈페이지를 통해 다양한 표현식들을 확인할 수 있었다. 우선 내가 구현한 간단한 기능에는 이 모든 것들이 필요 없으니 참고만 해두고 쉬운 것부터 살펴봤다. ^_^ 핫핫

1. 최초로 0이라는 숫자가 화면에 표시된다.
2. 더하기 버튼을 누르면 1이 증가한다.
3. 빼기 버튼을 누르면 1이 감소한다.

위와 같은 세가지 기능을 테스트해보자.

먼저 `1. 최초로 0이라는 숫자가 화면에 표시된다.`
는 App을 렌더링 시키고, 0이라는 엘리멘트를 가져오면 된다. (기본 test파일과 비슷하다.)

```js
test("0이 잘 보인다!", () => {
  render(<App />);
  expect(screen.getByText("0")).toBeInTheDocument();
});
```

이번에는 더하기, 빼기를 눌러보자.
이 두개는 비슷한 레벨로 묶을 수 있는데, 이럴 때는 각각 두개의 테스트를 하나의 describe로 묶을 수 있다.
(describe: 테스트 스위트(test suite)를 정의하는 함수)

>
>describe 함수는 주로 관련된 테스트들을 그룹화하고 설명하는 데 사용되며, test 함수는 각각의 테스트 케이스를 정의하는 데 사용된다!
>

조금 더 딴길로 새보자면,
describe는 아래와 같이 두가지 방식으로 사용될 수 있다.
```js
// 1. it 함수 사용
describe('테스트 단위 설명', () => {
  it('테스트 코드 설명', () => {
    // 테스트 코드
  });
});

// 2. test 함수 사용
describe('테스트 단위 설명', () => {
  test('테스트 코드 설명', () => {
    // 테스트 코드
  });
});
```
즉, `it`과 `test`는 동일한 기능을 수행하기 때문에 선호나 코드 컨벤션에 따라 사용하면 되겠다.

그럼 다시 돌아가 더하기와 빼기를 테스트해보자.

```js

describe("더하기와 빼기", () => {
  it("더하기를 누르면 1만큼 증가한다!", () => {
    render(<App />);

    const incrementButton = screen.getByRole("button", { name: "더하기" });
    fireEvent.click(incrementButton);

    expect(screen.getByText("1")).toBeInTheDocument();
  });
  it("빼기를 누르면 1만큼 감소한다!", () => {
    render(<App />);

    const decrementButton  = screen.getByRole("button", { name: "빼기" });
    fireEvent.click(decrementButton );

    expect(screen.getByText("-1")).toBeInTheDocument();
  });
});
```

App을 렌더링 시키고 (숫자는 0), 더하기 버튼을 누르면 1이 증가해 1이 화면에 표시되는 것과
App을 렌더링 시키고, 빼기 버튼을 누르면 1이 감소해 -1이 화면에 표시되는 것을 테스트할 수 있다.

전체 코드를 보면

```js
import { render, screen, fireEvent } from "@testing-library/react";
import App from "./App";

test("0이 잘 보인다!", () => {
  render(<App />);
  expect(screen.getByText("0")).toBeInTheDocument();
});

describe("더하기와 빼기", () => {
  it("더하기를 누르면 1만큼 증가한다!", () => {
    render(<App />);

    const incrementButton = screen.getByRole("button", { name: "더하기" });
    fireEvent.click(incrementButton);

    expect(screen.getByText("1")).toBeInTheDocument();
  });
  it("빼기를 누르면 1만큼 감소한다!", () => {
    render(<App />);

    const decrementButton  = screen.getByRole("button", { name: "빼기" });
    fireEvent.click(decrementButton );

    expect(screen.getByText("-1")).toBeInTheDocument();
  });
});
```
이렇게 되어있는데 최상단에 import한 `render, screen, fireEvent`은 각각
`render`: 컴포넌트를 렌더링하여 가상 DOM을 생성
`screen`: 렌더링된 컴포넌트 내에서 엘리먼트를 선택하고 검증하는 데 사용되는 함수들(getByLabelText, getByText, getByRole 등)을 제공
`fireEvent`:  DOM 이벤트를 시뮬레이션하는 함수들(click, change, submit 등)을 제공해 이벤트를 발생시킴
의 기능을 한다. 

### 4. 테스트 해보기

`npm test`로 테스트를 돌려보면
![](https://velog.velcdn.com/images/somda/post/031c0c5a-d6ce-41be-9b5b-ede6c1a5b75b/image.png)
이렇게 설명한 부분에 대해 얼마나 시간이 소요되었는지와 몇개의 테스트를 통과했는지 표시된다.

```js
  it("빼기를 누르면 1만큼 감소한다!", () => {
    render(<App />);

    const decrementButton  = screen.getByRole("button", { name: "빼기" });
    fireEvent.click(decrementButton );

    expect(screen.getByText("-3")).toBeInTheDocument();
  });
```
빼기 부분을 -1이 아니라 -3으로 고쳐 테스트 해보면
![](https://velog.velcdn.com/images/somda/post/4d14a71f-262d-486d-96d4-46105e209b88/image.png)
![](https://velog.velcdn.com/images/somda/post/dbc596b0-4516-4251-b64d-1d8535be8df8/image.png)

이렇게 어떤 테스트를 통과하지 못했는지와 통과 및 실패한 개수가 표시된다.


### 5. 테스트 언제할까?
 사실 이렇게 테스트 코드를 작성해보기 전에, 주변 프론트엔드 개발자들에게 현업에서도 테스트코드를 작성하느냐 물어본 적이 있는데 실제로는 거의 작성하지 못한다는 말을 들었었다. 아무래도 짧은 시간 내에 기능을 완성하는 것과 테스트 코드를 작성하는 것 모두를 잡기는 어려운 부분이 있으니 말이다.

 이전 프로젝트 경험을 돌이켜보면 
 처음부터 끝까지 시뮬레이션을 돌려보는 end to end 테스트를 많이 진행했었는데, 한 번에 왕창왕창 테스트하다보니 생각도 못한 문제가 한꺼번에 쏟아지기 일수였고, 또 작은 기능 하나를 고칠때마다 그 한 부분에 대해서만 테스트하기 힘들었었다.

 결론적으로, 모든 기능에 대해 테스트 코드를 작성하는 것은 현실적으로 어려운 문제일 것 같다. 하지만 핵심기능이나 계속해서 오류를 수정하게 되는 부분들은 테스트 코드 작성을 위해 오히려 개발 시간을 줄일 수 있지 않을까 싶다. 다음에는 조금 더 복잡한 기능에 대한 테스트 코드를 작성해봐야겠다!

 

 