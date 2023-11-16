지난번 다뤘던 클로저의 개념을 다시 짚어보자.
> 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 `클로저`라고 부른다.

```js
function outer(){
  const outerVar = "outer";
  
  funtion inner(){console.log(outerVar)}
  inner();
}

const inner = outer(); // outer 함수의 생명 주기 종료!
inner(); // "outer" -> 어라라 살아있네?
```

제일 궁금했던 부분은 이거였다.
왜 굳이 함수를 이중으로 만들어서! 외부 함수의 생명주기가 종료되었음에도! 외부 함수의 변수를 저장해야하는 걸까? 메모리 낭비 아닌가?


## 클로저(Closures)를 사용하는 이유

> 클로저를 통해 변수를 안전하게 보호하고 side effect를 억제할 수 있다.
> 다시 말해, 상태를 안전하게 변경하고 유지하기 위해 사용한다.

대표적인 증가 함수 예제를 통해 살펴보자.

```js
let num = 0; // 전역 변수
const increase = function(){
  return ++num; 
}

console.log(increase()); // 1
console.log(increase()); // 2
```
위의 함수는 잘 동작하지만, 전역으로 선언된 변수가 increase 함수 외에 다른 곳에서도 사용될 수 있으며, 변경될 수 있다는 문제점을 안고 있다.

그렇다고 increase함수 안에 선언하면 어떻게 될까?

```js
const increase = function(){
  let num = 0; // 지역 변수
  return ++num; 
}

console.log(increase()); // 1
console.log(increase()); // 1
```

increase 함수가 호출될 때마다 num이라는 변수가 다시 선언되고 0으로 초기화 되기 때문에, 원하는 기능이 동작하지 않는다.

이럴 때 우리는 클로저를 사용할 수 있다.

```js
const increase = (function(){
  let num = 0; 
  // 클로저 
  return function(){
  	return ++num;
  } 
}())

console.log(increase()); // 1
console.log(increase()); // 2
```
이렇게 클로저를 활용해서 변수를 숨겨버리면 전역 변수처럼 누구나 접근할 수 없어지고, _의도치 않게 값이 변경되는 일_을 막을 수 있다. 

이를 통해 Class 기반 객체지향 언어들의 private 속성을 흉내낼 수 있다.


### ⚠️ 메모리 점유의 문제는?

당연히 관련 변수를 기억하기 때문에 메모리를 점유하긴 한다. 다만,

```js
function outer(){
  const outerVar = "outer";
  const otherVar = "nothing"; // inner함수가 참조하지 않는 변수
  
  funtion inner(){console.log(outerVar)}
  inner();
}

const inner = outer(); 
inner();
```

위의 예시에서 `otherVar`같은 경우 `inner()`함수가 참조하지 않기 때문에, 똑똑한 자바스크립트 엔진은 이를 기억하지 않는다. 따라서 기억해야 할 식별자만 기억하고 있기 때문에 메모리 낭비라고 보기는 어렵다. 

### 올바르게 사용하는 법?

메모리 해제를 적절한 시점에 해주는 것, 클로저가 반복문일 때 주의할 것 등등... 여러가지 몇가지 주의점이 있었으나...

메모리 해제를 적절한 시점에 해주는 것 -> 자바스크립트 엔진 최적화가 잘 되면서 이제는 오히려 null 이나 undefined를 할당하는 행위가 오히려 비효율적이다 라는 주장도 있다. (팀원과 적절히 논의해서 사용하면 된다.)

클로저가 반복문일 때는 var의 선언과 초기화가 동시에 이뤄지는 문제를 신경 써야 했으나 ES6이후 let이 있으면서 이 문제도 쉽게 해결됐다.

따라서 클로저의 특징을 잘 이해하고 사용하는 것 그 자체가 클로저를 올바르게 사용하는 법이 아닐까 싶다.


### 클로저는 자바스크립트에만 있나요?

정답은 **No**.
클로저는 함수형 프로그래밍 언어라면 가지고 있는 특성이다!
함수형 프로그래밍 언어: 클로저(Clojure), 스칼라(Scala), 하스켈(Haskell) 등...



이렇게 클로저에 대해 알아봤다!
그리고 예시 코드를 다시 보자

```js
function outer(){
  const outerVar = "outer";
  const otherVar = "nothing"; // inner함수가 참조하지 않는 변수
  
  funtion inner(){console.log(outerVar)}
  inner();
}

const inner = outer(); 
inner();
```

사실 어딘가 익숙해보인다.

리액트로 코딩을 할 때, 
컴포넌트를 함수형으로 만들고 그 안에서 또 여러 함수들을 만드는데
그 내부 함수들은 엄연히 말하면 모두 클로저가 아닌가 🤔!

그러니 너무 어렵게 느껴진다면
그냥 내가 어떻게 상위 스코프의 변수를 그렇게 쉽게 바꿔왔는지 원리를 이해해보았다고 생각해도 될 것 같다.