본 포스팅은 React 공식 사이트의
[thinking-in-react](https://ko.react.dev/learn/thinking-in-react)와 [choosing-the-state-structure](https://ko.react.dev/learn/choosing-the-state-structure) 파트를 기반으로 작성되었습니다.

## state란
state는 일반 JavaScript 객체이며 react에게 리렌더링을 요청하는 하나의 트리거이기도 하다. 이 state를 더 '잘' 쓰기 위해 알아야할 것들을 정리해보았다.

### state 판별법

여러 데이터 중 어떤 것이 state이고 아닌지 판별하는 법은 다음과 같은 질문을 통해 해결할 수 있다.
>1. 시간이 지나도 변하지 않는가? 
2. 부모로부터 props로 전달되는가? 
3. 컴포넌트 안의 다른 state나 props를 통해 계산이 가능한가?

위에서 한 가지라도 해당되면 그 값은 **state가 아니다.**

예를 들어, 다음과 같이 3개의 상태값이 있을 때
```js
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  const [fullName, setFullName] = useState('');
```

`full name`의 경우
3번에 해당되기 때문에
아래와 같이 수정할 수 있다.
```js
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');

  const fullName = firstName + ' ' + lastName;
```

### state의 위치

state를 정의하는 위치의 경우 주로 공통 부모 혹은 공통 부모의 상위 컴포넌트에 배치하면 된다. 
적절한 컴포넌트가 없을 경우, state를 소유할 상위 컴포넌트를 추가할 것을 권장한다.


## state 설계 원칙

>1. 연관된 상태는 그룹화 해라 (Group related state) 
2. 상태의 모순을 피해라 (Avoid contradictions in state)
3. 불필요한 상태를 만들지 마라 (Avoid redundant state)
4. 상태의 중복을 피해라 (Avoid duplication in state)
5. 깊게 중첩된 상태를 피해라 (Avoid deeply nested state)

### 1. 연관된 상태는 그룹화 해라 (Group related state) 

커서의 위치를 표시하는 상태가 있다.
```js
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```
이 값은 관련있을 뿐 아니라, 커서의 위치가 바뀔 때마다 함께 바뀔 것이다. 
그렇다면 아래와 같이 바꾸는 것이 낫다.
```js
const [position, setPosition] = useState({ x: 0, y: 0 });
```

또한, 유저가 커스텀할 수 있는 값 등 갯수를 파악하기 힘든 경우도 배열이나 객체 형태로 만들어두는 것이 좋다.


### 2. 상태의 모순을 피해라 (Avoid contradictions in state)

입력한 text를 보내고 보내기가 완료되면 isSent true인 상태가 있다.
```js
  const [text, setText] = useState('');
  const [isSending, setIsSending] = useState(false);
  const [isSent, setIsSent] = useState(false);
```
이 경우는 값을 보내는 중인 것과 보낸 뒤의 상태, 이 두가지는 동시에 true일 수 없지만 별개로 관리하기 때문에 모두 true가 될 수 있는 문제점이 존재한다.

따라서 아예 text의 전송(입력 포함) 상태를 하나로 관리하는 편이 낫다. 
```js
  const [status, setStatus] = useState('typing');
```
status는 sending이 될 수도, sent가 될 수도 있지만 두개가 동시에 존재하지는 않게 된다.


### 3. 불필요한 상태를 만들지 마라 (Avoid redundant state)

 제일 처음 state를 판별하기 위한 예시에 들었듯,
lastname의 상태와 firstname의 상태가 있으면 fullname이라는 상태값을 별도로 두지 않아야 한다.

나아가, **props값을 그대로 state로 설정하는 것**에 유의해야 한다.
```js
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);
```

위와 같이 코드를 작성하게 되면, 부모 컴포넌트에서 messageColor가 바뀌더라도 자식 컴포넌트에서 바로 반영이 안 될 수 있다. 따라서 prop의 어떠한 업데이트도 무시하고 사용해야하는 경우에만 사용될 수 있다.


### 4. 상태의 중복을 피해라 (Avoid duplication in state)

```js
const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  const [selectedItem, setSelectedItem] = useState(
    items[0]
  );
  
  (...)
```
위의 코드에서 `selectedItem`는 선택된 아이템의 값을 보여준다. 하지만 `setItems`로 item 내부 값을 바꾸면 어떻게 될까?

선택된 아이템에는 바뀐 값이 바로 반영되지 않는다. `initialItems`라는 배열이 두개의 상태값에서 관리되고 있기 때문이다.
(실행 화면은 [공식 사이트](https://ko.react.dev/learn/choosing-the-state-structure)에서 확인할 수 있다.)

두 값의 sync를 유지하기 위해서는 아래와 같이 고치는 편이 낫다.

```js
  const [items, setItems] = useState(initialItems);
  const [selectedId, setSelectedId] = useState(0);

  const selectedItem = items.find(item =>
    item.id === selectedId
  );
```


### 5. 깊게 중첩된 상태를 피해라 (Avoid deeply nested state)


 마지막으로, 상태가 과하게 깊어지는 것은 피할 수 있다면 피하는 것이 좋다.
공식 사이트의 코드를 일부 가져와보면

```js
export const initialTravelPlan = {
  id: 0,
  title: '(Root)',
  childPlaces: [{
    id: 1,
    title: 'Earth',
    childPlaces: [{
      id: 2,
      title: 'Africa',
      childPlaces: [{
        id: 3,
        title: 'Botswana',
        childPlaces: []
      }, ...
```

`initialTravelPlan`이라는 객체 아래에 행성 -> 대륙 -> 국가명으로 이루어진 배열이 중첩되어있다.
각 배열 속 객체는 id, title, childPlaces를 가진 동일한 형태로 이루어져 있어

```js
export const initialTravelPlan = {
  0: {
    id: 0,
    title: '(Root)',
    childIds: [1, 42, 46],
  },
  1: {
    id: 1,
    title: 'Earth',
    childIds: [2, 10, 19, 26, 34]
  },
  2: {
    id: 2,
    title: 'Africa',
    childIds: [3, 4, 5, 6 , 7, 8, 9]
  }, 
  3: {
    id: 3,
    title: 'Botswana',
    childIds: []
  },
```
이와 같이 수정하면
상태를 변경할 때 불필요하게 전체를 복사할 필요가 없어진다.
(상태를 변경할 때 값을 바로 수정할 수 없다.)