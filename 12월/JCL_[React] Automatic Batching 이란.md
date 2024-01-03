# [React 18] Automatic Batching 

지난 번엔 react18 변경사항 중 하나인 useTransition 에 대해서 알아보았는데 오늘은 react18의 automatic batching(자동 배치)에 대해 알아보겠다. 

먼저, Batch 란 무엇인가?? 



## (1) Batching 

> Batching 이란 React가 더 나은 성능을 위해 여러 개의 state 업데이트를 하나의 리렌더링으로 묶는 것을 의미한다. 

예를 들면, 하나의 클릭 이벤트 안에 두 개의 state를 업데이트한다고 생각해보자. 버튼을 누를 때 마다 state는 두 번 변경되지만, react가 단 한 번의 렌더링만 할 수 있게 하는 것을 Batching 이라고 한다. 

```react 
function App() {
	const [count, setCount] = useState(0);
    const [flag, setFlag] = useState(false); 
    
    function handleClick() {
    	setCount((c) => c + 1); // (1) 
        setFlag((f) => !f);  // (2) 
        // (3) 
    }
	
    return (
    <div>
    	<button onClick={handleClick}>Next</button>
        <h1>{count}</h1>
    </div> 
    )
};
```

위의 코드에서 버튼을 클릭하면 (1), (2) 에서는 리렌더링이 일어나지 않고 setFlag 함수가 끝났을 때인 (3) 에서 리렌더링을 하게 된다. 이것이 배칭이다. 

하나만 더 예시를 보자면
```react
function App() {
	const [count, setCount] = useState(0);
    const handleClick = () => {
    	setCount((count) => count + 1);
        setCount((count) => count + 1);
        setCount((count) => count + 1);
    };
    
    useEffect(() => {
    	console.log("count", count); 
    }, [count]); 

} 
```

자바스크립트에 기반하여 생각한다면,
```bash 
count 1 
count 2 
count 3 
```
이런식으로 콘솔에 출력된다고 생각할 것이다.
```bash
count 3 
```
하지만 실제로는 하나만 찍힌다.

이렇게 배칭을 하게 되면, 불필요한 리렌더링을 줄이기 위해 성능이 좋고 컴포넌트가 반만 완료된 state를 렌더링하는 것을 방지한다. 즉, 렌더링 최적화를 위해 배칭이 있는 것이다. 

그런데 원래도 자동 배칭이 되었던 거 아닌가요? React 18에서 뭐가 달라졌다는 거죠 ..? 라고 생각할 수 있다! 어떤 점이 바뀌었는 지 지금부터 말해보겠다! 

## React 18 이전의 Batching 

> React 17 이하의의 과거 버전의 경우 이벤트 핸들러 내부에서는 이러한 자동 배치 작업이 이뤄지고 있었지만 Promise, setTimeout 같은 비동기 이벤트에서는 자동 배치가 이뤄지고 있지 않았다. 


```react
function App() {
	const [count, setCount] = useState(0);
    const [flag, setFlag] = useState(false); 
    
    function handleClick() {
    	fetchSomething().then(() => {
        	setCount((c) => c + 1); // (1) 
            setFlag((f) => !f);  // (2) 
            // (3)
        }); 
    }
	
    return (
    <div>
    	<button onClick={handleClick}>Next</button>
        <h1>{count}</h1>
    </div> 
    )
}
```
방금의 코드를 약간 변형시켜 보면 (1), (2) 에서 리렌더링이 발생하게 된다. 그 이유는 React가 클릭과 같은 브라우저 이벤트의 업데이트만 배칭을 해왔기 때문이고, 이 경우에는 fetch 콜백에서 이벤트가 핸들링이 완료된 이후에 state를 업데이트하기 때문이다. 


즉, React 17 이하의 과거 버전에서는 동기와 비동기 배치에 대한 작업에 일관성이 없었고, 이를 보완하기 위해 React 18 버전부터는 루트 컴포넌트를 createRoot 를 사용해서 만들면 동기, 비동기를 포함한 업데이트가 배치 작업으로 최적화할 수 있게 됐다. 

## (3) React 18에서의 Automatic Batching

![](https://velog.velcdn.com/images/chaedev3/post/0bb813a8-c8d3-4603-9bfb-d278ca765b9a/image.png)

먼저, react 공식문서에 따르면 react 18 에서는 createRoot 를 사용해야 한다고 적혀져 있다. 이렇게 바꾸게 되면 자동 배치가 활성화되어 리액트가 동기, 비동기, 이벤트 핸들러 등에 관계 없이 렌더링을 배치로 수행하게 된다.

```react 
function App() {
	const [count, setCount] = useState(0);
    const [flag, setFlag] = useState(false); 
    
    function handleClick() {
    	fetchSomething().then(() => {
        	setCount((c) => c + 1); // (1) 
            setFlag((f) => !f);  // (2) 
            // (3)
        }); 
    }
	
    return (
    <div>
    	<button onClick={handleClick}>Next</button>
        <h1>{count}</h1>
    </div> 
    )
}
```
React 18 에서는 (1), (2) 에서는 리렌더링을 하지 않고 콜백이 끝났을 때인 (3)에서 리렌더링을 하게 된다.


하지만, 우리가 react 18 로 바꾸면서 react 18 버전에서 작성한 기존 코드에 영향을 끼질 수도 있고 이러한 자동 배치를 이용하고 싶지 않을 수도 있다. 

그런 경우에는 flushSync 를 사용하면 된다!

```react 
import { flushSync } from "react-dom";

function handleClick() {
	flushSync(() => {
    	setCounter((c) => c + 1); 
    }); // (1) 
    flushSync(() => {
    	setFlag((f) => !f); 
    }); // (2) 
}
```
이렇게 하게 되면 (1), (2) 에서 리렌더링이 일어나게 된다. 


### 참고
https://17.reactjs.org/blog/2022/03/08/react-18-upgrade-guide.html
모던 리액트 딥다이브
https://immigration9.github.io/react/2021/06/12/automatic-batching-react.html 
https://happysisyphe.tistory.com/41 