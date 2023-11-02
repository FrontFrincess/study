# [FE] 클로저(Closures)란? (1)



프론트엔드 면접 단골 질문 중 하나! 
"클로저란 무엇인가요?"
제대로 답변하기 위해 내용을 배워보고 마지막에 답변화 해서 정리해보자!


## 클로저(Closures)란
 먼저 MDN에 정리된 내용을 보자면, 
> 클로저는 함수와 그 함수가 선언된 렉시컬 환경(lexical environment) 사이의 관계를 나타내는 개념이다. 클로저는 함수가 자신이 정의된 스코프 외부에서 변수를 참조할 수 있게 한다.

다소 모호하기도 하니...
문장을 하나씩 뜯어보자!
먼저 클로저는 함수와 그 함수가 선언된 **렉시컬 환경**사이의 관계를 나타낸다고 한다.

### 렉시컬 환경(lexical environment)?
 클로저를 이해하기 위해서는 렉시컬 환경에 대한 이해가 먼저 필요하다. 간단한 예시 코드로 실행컨텍스트와 렉시컬 환경을 먼저 이해해보자!

> 우선, **렉시컬 환경은 실행 컨텍스트를 구성하는 하나의 컴포넌트다.** 식별자와 식별자에 바인딩된 값을 저장하는 `Environment Record`와 상위 스코프에 대한 참조를 기록하는 `Outer Lexical Environment Reference`로 구성되어 있다.

```js
const globalVar = "global";

function outer() {
  const outerVar = "outer";

  function inner() {
    const innerVar = "inner";
    console.log(outerVar);
  }
  inner();
}

const outerVar = "fakeOuter";
outer();
```
다음과 같은 예시 코드가 실행될 때 실행 컨텍스트는 어떻게 쌓이고 실행되는지, 렉시컬 환경은 어떻게 생기는지 살펴보자.

_※ 기본적으로 객체 생성 -> 코드 평가 -> 실행 컨텍스트 생성 -> 렉시컬 환경 생성 -> 객체 환경 레코드 생성 -> 선언전 환경 레코드 생성 -> this 바인딩 -> 외부 렉시컬 환경에 대한 참조 결정 이라는 꽤 긴 과정을 함축한 이미지임에 유의해야 한다!_

![](https://velog.velcdn.com/images/somda/post/e5a05950-348a-4d85-a708-41603de9826a/image.png)
![](https://velog.velcdn.com/images/somda/post/ebf417c5-ef4f-43b5-b483-463e8307d246/image.png)

1. 전역 실행 컨텍스트와 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트를 실행한다.
전역 스코프기 때문에 외부 렉시컬 환경을 참조하지 않는다.

![](https://velog.velcdn.com/images/somda/post/09f5ba97-e320-4bdf-a8a4-70bdfc572f10/image.png)
![](https://velog.velcdn.com/images/somda/post/76911821-89e5-4e66-aaab-95b1f71f9418/image.png)

2. 전역 실행 컨텍스트에서 만난 outer 함수를 실행한다.
이에 맞게 outer함수에 대한 실행 컨텍스트와 렉시컬 환경을 생성한다.
상위 스코프인 전역 렉시컬 환경을 참조한다.

![](https://velog.velcdn.com/images/somda/post/5ef647f2-92f6-463a-81c5-20619362ea83/image.png)
![](https://velog.velcdn.com/images/somda/post/479819bb-66dc-4f62-b3a3-58f882ffd3e4/image.png)

3. outer함수 내에 있는 inner 함수를 실행한다.
역시 이에 맞게 inner함수에 대한 실행 컨텍스트와 렉시컬 환경을 생성한다.
상위 스코프인 outer 함수의 렉시컬 환경을 참조한다.

특히 inner 함수가 실행 될 때 outerVar라는 변수를 사용하고 있는데, 이 값은 inner함수 내에 선언된 값이 아니다. 따라서 내부를 먼저 탐색한 뒤 해당 값이 없다면 외부 환경으로 넘어가 outer함수의 렉시컬 환경에서 해당 변수를 찾게 되고, 해당 값을 출력한다.

![](https://velog.velcdn.com/images/somda/post/dc59f789-7e6a-4bc2-a900-02d2c9045563/image.png)
![](https://velog.velcdn.com/images/somda/post/a22cf619-ed9e-4f0d-94c1-1bf0ab4d1b2b/image.png)
![](https://velog.velcdn.com/images/somda/post/f2ff69a1-7afa-4190-84a4-25d69cded3ae/image.png)
이후 stack 구조의 원리대로 뒤에 들어온 실행 컨텍스트부터 순차적으로 실행을 완료해간다.


### 다시 돌아와서 그럼 클로저란?

> 클로저는 함수와 그 함수가 선언된 렉시컬 환경(lexical environment) 사이의 관계를 나타내는 개념이다. 클로저는 함수가 자신이 정의된 스코프 외부에서 변수를 참조할 수 있게 한다.

해당 문장에서 이번엔 두번째 문장을 보자! 
`클로저는 함수가 자신이 정의된 스코프 외부에서 변수를 참조할 수 있게 한다.`
언뜻 보면 당연히 상위 스코프를 참조한다는 소리 아닌가? 싶을 수도 있다.

위의 예시코드를 조금 바꿔보자!
```js
const globalVar = "global";

function outer() {
  const outerVar = "outer";

  return function inner() {
    const innerVar = "inner";
    console.log(outerVar);
  }
  inner();
}

const closure = outer(); // outer 함수를 호출하고 그 결과로 inner 함수를 반환

closure;
```

closure를 실행하면 
![](https://velog.velcdn.com/images/somda/post/b27ea21d-fefd-42a1-9b16-9696050e3c2d/image.png)
`const closure = outer();`해당 시점에서 `outer()`함수의 실행이 종료되었음에도 `outer()`함수의 지역 변수인 `outerVar`의 값을 잘 출력한다.

이것이 함수가 _자신이 정의된 스코프 외부에서 변수를 참조_할 수 있게 한다는 것을 의미한다.

또 코드를 또 조금 바꿔보자
![](https://velog.velcdn.com/images/somda/post/5c6d09ef-e2c0-4812-999d-6ac5026adf8d/image.png)

보면 closure가 호출되는 전역 단위에 outerVar라는 변수가 존재함에도 outer함수에서 할당된 outer가 출력되는 모습을 확인할 수 있다.

이것이 함수와 그 함수가 _선언된 렉시컬 환경(lexical environment) 사이의 관계_를 나타내는 개념임을 의미한다.


### 쉽게 정리해주세요!

그럼 이 개념들을 어떻게 쉽게 표현해볼까...했는데
모던 자바스크립트에서 아주 쉽게 정리해줬다! (야호!)

> 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있따. 이러한 중첩 함수를 `클로저`라고 부른다.

자, 이러면 또 궁금한 점들이 생긴다
1. 그럼 클로저를 생성하는 방법은 함수를 리턴하는 함수를 만들어 쓰는 것만 있나?
2. 클로서를 만들어서 쓰는 이유는 무엇이 있나?
3. 사용할 때 주의할 점은 없나?
4. 클로저는 자바스크립트 단독 개념인가? (미리 답하자면 No)
등등!

다소 어려운 개념이니만큼 확실히 정리하기 위해
2편으로 나누어 준비해보려 한다.

오늘도 화이팅~!