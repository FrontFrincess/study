![](https://velog.velcdn.com/images/yjohbjects/post/18037a90-6564-4a3b-bd70-79b0057f2735/image.png)


클래스형 컴포넌트와 함수형 컴포넌트의 차이를 알아보자!

클래스형 컴포넌트와 함수형 컴포넌트의 차이점을 알아보는 동시에, React의 생명주기에 대해 먼저 알아보려한다!

React 컴포넌트는 생명주기가 있는데, 컴포넌트가 실행되거나 업데이트되거나 제거가 될 때 특정한 이벤트가 발생한다.


# React의 생명주기
리액트 클래스 컴포넌트는 라이프 사이클 메서드를 사용하고,
함수형 컴포넌트는 Hook을 사용한다.

## 클래스 컴포넌트의 생명주기 (라이프 사이클 메서드)
![](https://velog.velcdn.com/images/yjohbjects/post/e7807ea0-e508-472f-95f0-929acd64b285/image.png)
클래스 컴포넌트는 생성 -> 업데이트 -> 제거의 생명주기를 갖는다.

### 생성(mounting)
컴포넌트의 인스턴스가 생성되어, DOM에 삽입될 때 순서대로 호출된다.

**constructor()**

- 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
- `this.props`, `this.state`에 접근할 수 있으며 리액트 요소를 반환
- setState를 사용할 수 없으며 DOM에 접근해선 안됨

**getDerivedStateFromProps()**
- props에 있는 값을 state에 동기화 시킬 때 사용하는 메서드

**render()**
- UI를 렌더링하는 메서드

**componentDidMount()**
- 컴포넌트가 웹 브라우저 상에 나타난 후 (렌더링을 마친 후) 호출하는 메서드
- 라이브러리나 프레임워크의 함수를 호출하거나 이벤트 등록
- `setTimeout`, `setInterval`과 같은 비동기 작업을 처리
- `setState` 호출도 이 메서드에서 호출 하는 경우가 많음

``` typescript
useEffect(() => {
	console.log("componentDidMount");
}, [])
```

### 업데이트 (updating)
props나 state가 변경되면 렌더링이 진행되며 순서대로 호출

**getDerivedStateFromProps()**
- 마운트 과정에서 호출, 업데이트가 시작되기 전에 호출된다
- props의 변화에 따라 state 값에도 변화를 주고 싶은 경우 사용

**shouldComponentUpdate()**
- props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드 (React.memo와 유사)
  - true: 다음 라이프 사이클 메서드를 계속 실행
  - false: 반환 작업 중지

**render()**
- 컴포넌트 렌더링

**getSnapshotBeforeUpdate()**
- 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드

**componentDidUpdate()**
- 컴포넌트 업데이트 작업이 끝난 후 호출하는 메서드 
(의존성 배열이 변할 때만 useEffect가 실행하는 것)


### 제거(unmounting)
컴포넌트를 DOM에서 제거하는 과정 (= 컴포넌트가 화면에서 사라지는 것)
**componentWillUnmount()**
- 컴포넌트를 DOM에서 제거할 때 실행 (화면에서 사라지기 직전에 호출)
- 주로 DOM에서 직접 등록했던 이벤트 제거
- `setTimeout`이 걸려있다면, `clearTimeout`을 통해 제거
- 외부 라이브러리를 사용하였다면, 해당 라이브러리의 dispose기능 호출
- 이 후 컴포넌트는 다시 렌더링 되지 않으므로, 여기서 setState() 호출 불가


## 함수형 컴포넌트의 생명주기 (Hook)
리액트의 Hook은 함수형 컴포넌트에서 React state와 생명주기 기능을 연동할 수 있게 해주는(`Hook into`) 함수이다. 
Hook은 클래스 안에서 동작하지 않고, 클래스 없이 React를 사용할 수 있게 한다.

### ❓ 왜 Hook이 도입되었을까
- 컴포넌트에서 상태관련 로직을 사용할 때 hook이전에 재사용 가능한 로직을 사용하기 위해서는, render props나 고차 컴포넌트와 같은 패턴을 사용했는데, 이런 패턴은 코드의 추적을 어렵게 만들었다.
-> hook을 활용하면 상태 관련 로직을 추상화해 독립적인 테스트와 재사용이 가능해 레이어 변화 없이 재사용할 수 있다.
- 라이프 사이클 메서드 기반이 아닌 로직 기반으로 나눌 수 있어서 컴포넌트를 함수단위로 잘게 쪼갤 수 있다는 이점
-> 라이프사이클 메서드에는 관련 없는 로직이 자주 섞여 들어가는데, 이로인해 버그가 쉽게 발생하고 무결성을 쉽게 해친다.

### 🔨 Hook 사용 규칙

☝️ **최상위에서만 Hook을 호출**할 것
⠀⠀⠀반복문, 조건문, 중첩된 함수 내에서 hook을 실행하면 안됨🙅👊
⠀⠀⠀=> 이 규칙을 따르면 컴포넌트가 렌더링될 때마다 항상 동일한 순서로 hook이 호출되는 것이 보장된다.
✌️** 리액트 함수 컴포넌트에서만 hook을 호출 **
⠀⠀⠀(일반 JS함수에서는 hook 호출해서는 안됨🙅👊)

📌 이 두가지 규칙을 강제하는 플러그인은 `eslint-plugin-react-hooks` (Create React App에 포함)


### 생명주기에 해당하는 Hook
#### constructor()
: useState hook을 사용하여 초기상태를 설정해줄 수 있다
```typescript
const ConstructorExample = () => {
  const [example, setExample] = useState('example');
}
```

#### getDrivedStateFromProps()
: useState hook을 사용하여 렌더링 중 업데이트를 할 수 있다
```typescript
function ScrollView({row}) {
  const [isScrollingDown, setIsScrollingDown] = useState(false);
  const [prevRow, setPrevRow] = useState(null);

  if (row !== prevRow) {
    // 마지막 렌더링 이후 행이 변경
    // isScrollingDown을 업데이트
    setIsScrollingDown(prevRow !== null && row > prevRow);
    setPrevRow(row);
  }

  return `Scrolling down: ${isScrollingDown}`;
}
```

#### render()
: render()대신 컴포넌트를 렌더링한다
```typescript
const RenderExample = () => {
  return <div>렌더링 될 컴포넌트</div>
}
```

#### componentDidMount()
: useEffect를 활용하여 의존성배열[]을 비우고 구현할 수 있다
```typescript
const ComponentDidMountExample = () => {
  useEffect(() => {
    console.log('componentDidMount');
  }, []);
```

#### shouldComponentUpdate()
: props는 React.memo, state는 useMemo를 활용하여 렌더링 성능을 개선한다
```typescript
// props
const shouldComponentUpdate() = React.memo(() => {
  ...
},
  (prevProps, nextProps) => {
    return nextProps.value === prevProps.value
  }
  )
// state
```

#### getSnapshotBeforeUpdate()
: 함수형에서는 아직 대체할만한  hook이 없다
#### ComponentDidUpdate()
: useEffect를 활용하여 []의존성 배열을 활용하여 조회할 수 있다
```typescript
const ComponentDidUpdateExample = () => {
  useEffect(() => {
    console.log('componentDidUpdate');
  }, [prop]);
```
#### componentWillUnmount()
: useEffect CleanUp 함수를 통해 구현한다
(CleanUp 함수란, parameter로 넣은 함수의 return 함수)
```typescript
const ComponentWillUnmountExample = () => {
  const [count, setCount] = useState(0)
  
  useEffect(() => {
    console.log(count, 'mount')
    return () => {
      console.log(count, 'mount')
    }
  }, [count]);
```

# 결론
함수형 컴포넌트는 클래스형 컴포넌트보다 선언하기 훨씬 쉽고,
메모리 자원을 클래스형 컴포넌트보다 덜 사용한다.
때문에 빌드한 결과물의 크기 역시 클래스형 컴포넌트보다 작다.

현재는 많은 사람들이 함수형 컴포넌트로 개발을 진행하지만, 기존에 클래스형 컴포넌트로 개발이 진행된 곳이 있을 수 있기 때문에, 개념정도는 알아두는 것이 좋겠다.

_**참고**
https://whales.tistory.com/146
https://soopiri.tistory.com/23_