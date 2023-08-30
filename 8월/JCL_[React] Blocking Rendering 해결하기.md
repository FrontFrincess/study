## [React] Blocking Rendering 해결하기 (feat. startTransition, Debounce, Throttle) 

React 18 변경 사항에 대해 알아보다가 startTransition 이라는 걸 알게 되었다!! 변경사항에 대해 모두 다 포스팅하면 좋겠지만 .. 머리가 안좋아서 하나씩 차근차근 알아보도록 해야겠다.. 오늘은 startTransition 에 대해서 알아보도록 하자!! 

<br />

## ⭐  Blocking Rendering 문제 

> 한 번 렌더링 연산이 시작되면 멈출 수 없기 때문에 많은 화면을 업데이트 하는 경우에 페이지 지연 현상이 발생하는 것을 말함 

이 문제를 알아보기 위해 간단한 코드를 작성해보았다. 

```javascript
function MainPage() {
  const [boxCount, setBoxCount] = useState(0);

  const handleUpdate = ({ target }) => {
    setBoxCount(target.value.length * 2);
  };
  return (
    <>
      <Title>기본</Title>
      <Input type="text" onChange={handleUpdate} />
      <BoxList count={boxCount} />
    </>
  );
}

export default MainPage;
```

입력하는 값의 길이에 따라서 Box(빨리 출력하기 위해 2배씩)를 출력해주는 코드를 짜보았다. 

![Animation1](https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/f45c7333-d979-4b5c-b558-247d5ef312a2)

화면을 보면 내가 마우스로 동그라미를 그리는 순간에 'ㅊㅊ' 라는 글자를 썼는데 그 보다 더 느리게 뜨는 걸 알 수 있다. 이런 게 바로 Block Rendering 문제다!! 

## <br/>**⭐ startTransition** 

이러한 문제를 개선하기 위해 React 18에서 등장한 것이 바로 이 startTransition 이다. 

> startTransition 을 이용하면 우선순위를 낮출 수 있다. 즉, startTransition 으로 감싼 부분의 우선순위가 낮아지므로 그 외의 것들의 우선순위는 높아진다. 

방금 짠 코드로 봤을 때, 강아지 아이콘 출력의 우선순위를 낮춰주면 input 창의 우선순위는 높아지게 된다는 것이다. 또한, useTransition 의 isPending 을 이용해서 Pending 상태를 나타낼 수도 있다. 

```javascript
function StartTransitionPage() {
  const [boxCount, setBoxCount] = useState(0);
  // startTransition 적용 
  const handleUpdate = ({ target }) => {
    startTransition(() => {
      setBoxCount(target.value.length * 2);
    });
  };
  return (
    <>
      <Title>StartTransition</Title>
      <Input type="text" onChange={handleUpdate} />
      <BoxList count={boxCount} />
    </>
  );
}

export default StartTransitionPage;
```

![Animation2](https://github.com/chaedev3/TIL/assets/109324466/1e778300-cf96-4033-bfed-b92b88276679)

-> 이렇게 startTransition을 사용하게 되면 input 창의 우선순위가 높아지기 때문에 입력을 했을 때 바로 뜨게 된다. 

<br />

useTransition의 isPending 을 추가하게 되면, 처리 중일 때 이렇게 뜨게 된다. 

```javascript
function StartTransitionPage() {
  // isPending 추가! 
  const [isPending, startTransition] = useTransition();
  const [boxCount, setBoxCount] = useState(0);
  const handleUpdate = ({ target }) => {
    startTransition(() => {
      setBoxCount(target.value.length * 2);
    });
  };
  return (
    <>
      <Title>StartTransition</Title>
      <Input type="text" onChange={handleUpdate} />
      {isPending && <h1>Pending@!!</h1>}
      <BoxList count={boxCount} />
    </>
  );
}

export default StartTransitionPage;
```

![Animation2](https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/aa61111b-c8ca-4942-8c4f-80858fc0c55b)

##### 그렇다면 이런 건 어떻게 가능할까 ?!  

그것은 React 18 버전에서는 렌더링을 하는 와중에도 더 급한 일이 생기면 그것을 먼저 처리할 수 있게 되었기 때문이다. 더 깊게 알아보고 싶다면 React 18 에 도입된 '동시성' 에 대해 알아보면 좋을 것 같다. (다음 주제로 할지도..? )  



##### 그렇다면 이미 존재했던 Debounce 와 Throttle 에 대해 알아보자!!  

> Debounce 와 Throttle 은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.  

input 태그 같은 곳에서 값을 입력할 때마다 API 가 호출이 된다면 유료 API의 경우 호출할 때마다 비용이 발생하기 때문에 줄여주는 것이 필요한데 이럴 때 사용하는 것이 바로 Debounce, Throttle 이다. 

<br />

## ⭐ Debounce

> 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후에 이벤트 핸들러가 한 번만 호출되도록 한다. 

<img width="631" alt="mjs-41-2" src="https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/03bf2a45-64b4-4549-ba3b-d0b1c262c8e5">

예시를 통해 살펴보자면, 

```javascript
function DebouncePage() {
  const [boxCount, setBoxCount] = useState(0);
  // Debounce 설정 
  const handleUpdate = _.debounce((e) => {
    setBoxCount(e.target.value.length * 2);
  }, 1000);

  return (
    <>
      <Title>Debounce</Title>
      <Input type="text" onChange={handleUpdate} />
      <BoxList count={boxCount} />
    </>
  );
}

export default DebouncePage;
```

lodash 를 통해서 debounce 를 구현할 수 있고, delay 되는 시간을 설정할 수 있다.  (꼭 lodash 를 써야 하는 것은 아니다. Underscore 의 debounce 를 이용하는 방법이나 직접 구현하는 방법 등등 .. ) 

![Animation4 ](https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/99779cc9-e9d7-4fd4-a294-ad2cc923c523)

즉, 위와 같이 input 입력을 멈춘 후 delay 타임이 지난 후에 강아지 아이콘이 출력이 되는 걸 알 수 있다. Debounce 같은 경우 주로 resize 이벤트 처리, input 요소에 입력된 값으로 ajax 요청하는 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지 처리 등에 사용된다. 

<br />

## ⭐ Throttle 

> 짧은 시간 간격으로 이벤트가 연속해서 발생하더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 하는 것이다. 

![mjs-41-3](https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/40be8d6f-c569-402d-aa9f-3edaca24ca80)

즉, Debounce 는 이벤트가 멈춘 후에 딜레이 시간이 지나면 호출하지만, Throttle 은 이벤트가 발생하고 있더라도 일정 시간이 지나면 호출이 되는 것이다. 

예를 들어 살펴보자면.. 

```javascript
function ThrottlePage() {
  const [boxCount, setBoxCount] = useState(0);
  const throttle = _.throttle((e) => {
    console.log(e.target.value);
    // setBoxCount(e.target.value.length * 2);
  }, 500);

  return (
    <>
      <Title>Throttle</Title>
      <Input type="text" onChange={throttle} />
      <BoxList count={boxCount} />
    </>
  );
}

export default ThrottlePage;
```

- 강아지 아이콘 출력을 하면 throttle 이 동작을 안해서 (?) 일단 console 만 찍어보겠다. 

![Animation4 ](https://github.com/chaedev3/useTransition_debounce_throttle_example/assets/109324466/c5fcc65f-48ea-4e15-b37a-aaf701519cc7)



이렇게 끊기지 않고 입력을 하더라도 일정 시간 간격으로 호출을 하는 것을 알 수 있다. 

Throttle 의 경우에는 scroll 이벤트 처리나 무한 스크롤 UI 구현 등에 유용하게 사용된다. 

##### <br />

일단, 앞서 살펴봤던 디바운스의 경우에는 입력이 멈추면 한 번에 호출하는 것이기 때문에 문제 상황을 뒤로 미루는 꼴이 되는 것이다. 

또한, 쓰로틀의 경우에는 입력 주기 동안 계속 입력을 한다면 모르겠지만 띄엄띄엄 입력을 한다면 의미 없는 기다림이 생기는 것이다. 

반면, startTransition을 이용하면 화면 업데이트 중에도 사용자 입력을 계속 받을 수가 있다. 즉, 디바운스 & 스로틀과 비교하면 비어있던 시간들이 사라지고 더 나은 사용자 경험을 줄 수 있을 것이다. 

<br/>

#### **📒**  참고 

- https://www.youtube.com/watch?v=focpJqfSu4k 

- 모던 자바스크립트 딥 다이브 

- https://webclub.tistory.com/607 

- https://itprogramming119.tistory.com/entry/React-Debounce-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%B0%8F-%EC%98%88%EC%A0%9C 

<br/>

게시물에 사용된 코드는 Github 에 올려두었습니다. 

https://github.com/chaedev3/useTransition_debounce_throttle_example  

