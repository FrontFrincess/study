타입스크립트를 사용할 때 가장 고마운 존재이자 번거로운 존재인 타입!(당연함)

타입과 인터페이스의 차이에 대해 간단히 알아보고,
어떻게 상황에 맞게 쏙쏙 사용할 수 있을지 알아보자!

## Type과 Interface의 차이
![](https://velog.velcdn.com/images/somda/post/2a850232-f7a9-45b4-a877-cf7feb0b5563/image.png)
 우선, TypeScript 문서에서부터 Type과 Interface의 차이는 그렇게 크지 않다고 말하고 있다. (뒷 내용은 잠시 무시한다.)
 ![](https://velog.velcdn.com/images/somda/post/a6ae2917-f97c-4e7f-84e5-66b50996a089/image.png)
 그리고 타입이 필요하기 전까진 웬만하면 인터페이스를 사용할 것을 권장한다.

그렇다면 타입이 필요한 상황만 몇가지 알아보면 된다!

몇 개의 케이스를 통해서 언제 무엇을 쓸 지 araboza.

### Case1. Extends로 확장이 필요할 때

이미 만들어둔 타입과 똑같은 내용에 몇 개만 더 추가하고 싶다면? extends를 통해 type을 확장해서 사용하면 된다.
그리고 이때는 interface를 통해 확장하는 것이 좋다. interface는 캐싱의 장점을 가지고 있기 때문이기도 하다.

> 기존에는 type으로는 extends를 사용할 수 없었지만 현재는 일부 지원한다. 하지만 이 때 type은 `|`을 포함하지 않는 등 정적인 타입이어야 한다.

간단한 예시는 다음과 같다.

```ts
export interface FruitType {
  taste: string
  price: number
}

interface AppleType extends FruitType {
  isRealApple: boolean
}
```


### Case2. 조건부로 타입 지정하기

**2-1. Union Type(합 타입)**

위에서 type을 확장하려면 `|`을 포함하지 않는 등... 이라는 언급을 했다. 
`|`조건부 연산은 Union Type, 즉 합 타입을 만들 때 사용하는데
type과 함께 사용하며 다음과 같은 경우다. 

`type price = string | number`

어 나 이거 인터페이스에서도 썼는데? 라고 한다면
```ts
interface FruitType {
  price: string | number
}
```
의 케이스 였을 것이다.

`interface price {string | number} ` 이렇게 사용하는 것이 불가능하다는 소리니 참고하자.

**2-2. Intersection Type(교차 타입)**

합 타입이 두 타입 중 하나를 뜻했다면 교차 타입은 두 타입의 속성을 모두 가진다.

2-1에서 뭔가 눈치를 챈 사람도 있을텐데
```ts
interface Price {string & number}
type PriceType = string & number
```

둘 중에 어떤 걸 사용해야할까?

정답은

```ts
interface Price {string & number} // X
type PriceType = string & number  // O
```
이다.

또는(|)이나 그리고(&)를 연산으로 생각했다면 유추할 수 있었을 것이다. 

실제로 이번에 회사에서 코드를 짜면서, 아래와 같은 복잡한 상황을 마주하게 되었고 (변수명은 일부 변경하였다)
인터페이스를 주로 사용하지만 이 상황만은 불가피하게 type을 사용하게 되었다.
인터페이스로 하려니까 어쩐지 안 되더라..!

```ts
type PaperItemProps = {
  paper: PaperType | PaperLogType
  onClick?: Function
} & (PaperType["status"] extends "cut" ? { paper: PaperLogType } : { paper: PaperType })
```

해당 케이스를 간단히 보면
paper는 PaperType일 수도, PaperLogType일 수도 있다. 그런데 넘겨주는 입장에서는 PaperType으로 정의해서 내려주고 있는 상태라,
우선 PaperType의 status를 통해 해당 값이 cut인지 아닌지를 판별하고, PaperLogType이나 PaperType 중 하나를 할당한다.


### 유의할 점

 대부분 위의 케이스를 벗어나면 인터페이스를 주로 사용하게 될 것이다. 그럼 글을 마무리 하기 전에 인터페이스의 특징 딱 두 개만 더 짚어보자.

 * 인터페이스는 객체형태에 이름을 붙이는 것이다. 즉, 객체 형태에서만 사용할 수 있다.

 * 인터페이스는 선언된 인터페이스를 합쳐버리는 특징이 있다.

특히 두번째 특징 같은 경우, 아래와 같이 합쳐지기 때문에 의도하지 않은 상황을 만들지 않기 위해 변수명에 유의하자.

```ts
interface Fruit {
  color: string
}

interface Fruit {
  price: number
}

// Error 발생! 필수값 Price가 누락된 것으로 판단한다.
const apple: Fruit = {
  color: "red"
}
```




참고링크

[Differences Between Type Aliases and Interfaces](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)
[TypeScript:type과 interface는 어떻게 다를까](https://medium.com/hcleedev/typescript-type%EA%B3%BC-interface%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8B%A4%EB%A5%BC%EA%B9%8C-c4ca65a0257)