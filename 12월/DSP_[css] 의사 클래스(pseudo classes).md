아침 출근길에 커리어리 추천글을 훑어보는 게 소소한 루틴이 되어가는 요즘... 쉬는 동안 안 본 밀려있는 추천글들 중 재밌는 글을 발견했다.

바로 firefox에서도 `:has` 사용 가능!
[관련 아티클](https://robbowen.digital/wrote-about/locking-scroll-with-has/)

그 말은 firefox 말고 다른 브라우저는 이미 지원을 하고 있다는 거 아닌가!
`:hover` 정도는 실제로 사용했던 것 같지만 `:has`는 듣도 보도 못했는데..!

관련해서 찾아보다 보니 내가 몰랐던 무수히 많은 `:블라블라`들...
그 중 잘 안 썼지만 잘 쓸 수 있을 것 같은! 몇 가지를 정리해보자.

## 먼저, 의사 클래스란?

Q1. css에서 사용되는`:`은 뭐야?
A. css의 의사 (not doctor yes pseudo) class라고 한다.

Q2. 의사..? 클래스?
A. 의사 클래스의 뜻이 직관적으로 와닿지 않는다면 **가상 클래스**라고 이해해도 된다.
가상 클래스는 *HTML 요소가 어떤 특정한 상태일 때 해당 요소를 선택하겠다는 뜻*이다.

```javascript
<button classname="btn">
  Temp button
</button>

// css
.btn:hover {
  background-color: aqua
}
```

예를 들어 위와 같은 코드는

`:hover`라는 의사클래스를 통해 마우스가 올라간 상태일 때
`.` 이라는 클래스 선택자를 통해 btn이라는 이름을 가진 요소를 선택하고
배경색을 아쿠아색으로 하겠다!

라는 의미를 가진다.

> 참고!
> `::` 이렇게 콜론이 두개 있는 건 의사 요소(가상 요소) 라고 부른다. 요소의 특정 부분에 대한 스타일을 만드는 데 사용된다.
> CSS의 다양한 선택자로는 `.`, `#` 등이 있으며 자식 결합자 `>` 등 다양한 결합자도 존재한다!
> 상세 내용: [MDN CSS 선택자](https://developer.mozilla.org/ko/docs/Web/CSS/CSS_selectors)

### `:first-child`와 `:first-of-type`

first, last 뿐 아니라 nth까지 포함하는 다양한 가상 클래스들. 그 중 대표로 두개만 뽑아 왔다.

`:first-child`: 자식 요소 중 첫 번째에 위치하는 요소를 모두 선택한다.
`:first-of-type`: 자식 요소 중 첫 번째로 등장하는 요소를 모두 선택한다.

코드로 봐보자!

```js
<div className="parent">
    <p>First Paragraph</p>
	<p>Second Paragraph</p>
	<span>First Span</span>
	<p>Third Paragraph</p>
</div>

// css
.parent :first-child {
  color: red;
}
.parent :first-of-type {
  background-color: yellow;
}
```

`first-child`는 첫번째 요소를 선택하기 때문에 가장 첫번째 자식만 빨간 색으로 글씨색이 변했고
`first-of-type`은 타입이 첫번째로 등장하면 찾아내기 때문에 p태그의 첫번째와 span태그의 첫번째 요소를 선택했다.
![](https://velog.velcdn.com/images/somda/post/dbd78351-da31-446a-a41a-c0d1119b2568/image.png)

### `:not()`

이름에서도 알 수 있듯 부정을 의미한다. 괄호 안에 인수를 받으며 해당 요소를 부정한다.

바로 코드로 봐보자.

```js
<div className="parent2">
    <div>item1</div>
	<div>item2</div>
	<div className="not-item">item3</div>
	<div>item4</div>
</div>

// css
.parent2 {
  background-color: antiquewhite;
  padding: 10px;
}

.parent2 div:not(.not-item){
  background-color: white;
  margin: 5px;
}
```

not에 해당하는 세번째 요소를 제외하고 나머지 div 요소에만 스타일이 적용된다.

![](https://velog.velcdn.com/images/somda/post/6cdf1c17-6fc2-40fe-bf6b-0e3685e24e2e/image.png)

### `:where`과 `:is`

`where`과 `is`는 유사점과 차이점을 위주로 살펴보자.
우선 이 녀석들이 쓰이는 상황은 아래와 같다.

```js
<article>
  <h3>Header3</h3>
  <h4>Header4</h4>
  <h6>Header6</h6>
  <p>Paragraph</p>
</article>
```

이 중 h4, h6, p 태그에 동일한 스타일을 적용하고 싶다면 어떻게 할까?

```css
article h4,
article h6,
article p {
  color: blue;
}
```

이렇게 작성하면 article이 반복되는 걸 확인할 수 있다.
이렇게 반복이 싫을 때! where 이나 is를 사용하면 깔끔하게 정리할 수 있다.

```css
article :where(h4, h6, p) {
  color: blue;
}

// 또는
article :is(h4, h6, p) {
  color: blue;
}
```

적용하면 아래와 같다.
![](https://velog.velcdn.com/images/somda/post/3fd4cade-08d0-4b5e-b143-dadb19092155/image.png)

그럼 `where`과 `is`의 차이는? 바로 **명시도**다!
`where`은 명시도가 0인 데에 반해
`is`는 높은 명시도를 가져 우선권을 가진다.

```css
article :where(h4, h6, p) {
  color: blue;
}

article :is(p) {
  color: green;
}
```

이렇게 `article p`에 대해 스타일을 적용하면
`is`에 작성된 green이 우선권을 가져 아래와 같이 반영된다.
![](https://velog.velcdn.com/images/somda/post/56d44a1f-1bc4-444b-8ff7-cee8d4c43ba2/image.png)

### `:has`

마지막은 내 의문이 시발점이 된 녀석이다. 가장 최근에 등장했으며 모든 브라우저에서 지원한다.
`a :has(x)`가 있을 때, `a`가 `x`를 가지고 있는지를 판별한다고 생각하면 되는데, 그 판별 기준을 아주 다양하게 지정할 수 있다.

```js
<div className="parent3">
  <div className="has-div">
    <p>Paragraph</p>
    <h6>Header6</h6>
  </div>

  <h6>Header6</h6>
</div>
```

먼저 `has(h6)` 이나 `has(>h6)`처럼 사용하면?
parent3 div가 자식(자손)으로 h6을 가지고 있기 때문에 color가 적용 된다.
![](https://velog.velcdn.com/images/somda/post/77920e83-2f6c-4812-a3f3-0f285231f1d7/image.png)

```css
.parent3:has(h6) {
  color: burlywood;
}
```

다음으로, `has(+h6)`처럼 사용하면?
`+`는 형제 선택자기 때문에 has-div녀석에게 스타일을 줘 보았다.
![](https://velog.velcdn.com/images/somda/post/c02c63e1-3838-47c5-92ac-cb88c096af3e/image.png)

```css
.has-div:has(+ h6) {
  color: blue;
}
```

`>`, `+` 외에도 다양한 선택자들을 활용해 여러 방식으로 활용할 수 있다.
`not` 등 다른 의사 클래스들과의 조합도 당연히 가능하다.

```css
.parent3:not(:has(h6)) {
  color: red;
}
```

이런 식으로 parent3가 h6 요소를 가지고 있지 않다면!
스타일이 변경되는 것이다.
위의 코드에서는 포함하고 있기 때문에 스타일이 적용되지 않는다 :)

이 외에도 다양한 가상 클래스가 있으니 더 궁금하다면 mdn을 살펴보는 것을 추천한다!
https://developer.mozilla.org/ko/docs/Web/CSS/Pseudo-classes
