# 정규 표현식(Regular Expression) 

<br />

정규 표현식은 회원가입 유효성 검사에 사용되는 정도는 알고 있었지만 모든 프로젝트를 소셜 로그인으로만 진행을 했기에 딱히 쓸 일이 없었다!! 하지만 알아두면 좋을 것 같아 알아보기로 하자.. 

<br />

먼저 정규 표현식이 사용되는 예시는 다음과 같다. 

- 각각 다른 포맷으로 저장된 엄청나게 많은 전화번호 데이터를 추출해야 할 때 
- 사용자가 입력한 이메일, 휴대폰 번호, IP 주소 등이 올바른지 검증하고 싶을 때 
- 코드에서 특정 변수의 이름을 치환하고 싶지만, 해당 변수의 이름을 포함하고 있는 함수는 제외하고 싶을 때 
- 특정 조건과 위치에 따라서 문자열에 포함된 공백이나 특수문자를 제거하고 싶을 때 

.. 등등 

<br />

React 에서의 사용 예시를 두 가지 정도 먼저 보고 정규 표현식에 대해 자세하게 알아보자!! 

##### (1) 유효성 검사 

- 사실 React 에는 react-hook-form 이라는 라이브러리가 있어서 유효성 검사할 때 이 라이브러리를 사용했던 것 같은데 정규표현식을 이용하여 유효성 검사를 해보겠다. 
- 그냥패스워드를 받는 입력 폼을 구현해보자. 패스워드는 8-20 자리를 가져야 하며 숫자, 영문, 특수문자를 모두 포함해야한다고 쳐보자. 

````javascript
<form action="" onSubmit={handleClick}>
	<input 
		type="password"
		name="password"
		placeholder="비밀번호 입력 (영문, 숫자, 특수문자 조합 8 - 20 자리)"
		value={form.password || ''}
		onChange={onLogin} 
	/> 
	{!pwValid && '비밀번호는 영문, 숫자, 특수문자 조합 8 - 20 글자입니다.'}
	<button 
		type='text'
		onClick={handleClick}
		disabled={!pwValid} 
	>
		로그인 
	</button> 
</form> 
````

- 여기서 pw 가 유효한지 안한지 판별하기 위해서는 정규 표현식을 사용해야 한다 

```javascript
const pwRegExp = /^(?=.*[a-zA-Z])(?=.*[!@#$%^*+=-])(?=.*[0-9]).{8, 20}$/; 

const pwValid = pwRegExp.test(form.password); 
```

<br/>

##### (2) 검색 

- 지금까지 프로젝트를 진행하면서 검색을 구현해야 할 일이 있을 때 모두 소문자로 변환해서 검색어를 포함하는 것들을 filtering 하는 식으로 구현하였다.  

```javascript
jconst bookList = [
	{id: 1, title: '세이노의 가르침'}, 
	{id: 2, title: '메리골드 마음 세탁소'}, 
	{id: 3, title: '도둑맞은 집중력'},
	{id: 4, title: 'The Giver'}, 
	{id: 5, title: 'Wonder'}, 
	... 
]
```

- includes  

```javascript
bookList.filter(item => item.title.toLowerCase().includes(search.toLowerCase())); 
```

> 찾고자 하는 것을 search 라고 한다면  밑의 코드처럼 검색어와 data 에 있는 것들을 다 소문자로 바꿔주고 검색어 포함 여부를 필터링하면 된다.  

<br/>

- 정규표현식

```javascript
bookList.filter(item => {
	const regExp = new RegExp(search, 'gi')  // g 는 전역 검색, i 는 대소문자 구분 없이 검색하겠다는 뜻 
	return item.title.match(regExp)  // 패턴(regExp)를 검색해서 매칭되는 것만 반환하겠다는 뜻 
}
```

<br />

https://bluewings.github.io/unobstructed-hangul-regular-expression/ 

- 이건 한글 자동완성을 위한 정규식 라이브러리인데 쓰면 좋을 것 같아 가져와봤다. 궁금하신 분들은 링크 클릭 .. 

<br />

## 1. 정규표현식

> 정규 표현식은 문자열에서 특정 문자 조합을 찾기 위한 패턴이다. 정규표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있는 장점이 있지만 가독성이 좋지 않다는 문제가 있다. 

<br />

예를 들면, 우리가 전화번호값을 입력을 받는다고 생각을 했을 때 그냥 input 값으로 놓게 되면 사용자가 어떤 형식을 입력하든 다 허용을 하게 된다. 만약 000-0000-0000 형태로 숫자와 - 로 이루어진 값만 받고 싶을 때 사용하는 것이 바로 정규표현식이다!! 

```javascript
const regExp = /^\d{3}-\d{4}-\d{4}$/; 
```

-> 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한 것이다. 숫자 3개 - 숫자 4개 - 숫자 4개를 받겠다는 것인데 잘 모르겠다면 지금부터 천천히 알아보자!! 

<br />

정규 표현식의 객체(RegExp 객체)를 생성하기 위한 방식은 2가지가 있는데 **1) 리터럴(Literal) 방식**  **2) RegExp 생성자 함수 방식** 이다.  

<br />

#### 1) 정규 표현식 리터럴 

정규 표현식 리터럴은 <u>패턴(pattern)과 플래그(flag)</u>로 구성이 된다.  

![주석 2023-08-08 125351](https://github.com/chaedev3/todo_list_dark_mode/assets/109324466/b03049de-748b-4264-a7bd-a9fa824415da)

- 정규 표현식 리터럴은 슬래시(/) 기호로 시작하여, 슬래시(/) 기호로 끝나며 따옴표를 사용하지 않는다. 
- 패턴(pattern) : 문자열의 <u>일정한 규칙을 표현</u>하기 위해 사용한다. 
- 플래그(flag) :  정규 표현식의 <u>검색 방식을 설정</u>하기 위해 사용한다.  

```javascript
const target = 'Front princesses are very smart';
const regExp = /front/i;
regExp.test(target);   // true (test method 는 매칭 결과를 Boolean 값으로 제공한다.)
```

-> 여기서 패턴은 front , 플래그는 i 이고 i는 대소문자를 구별하지 않고 검색하겠다는 의미이다. 

<br />

#### 2) RegExp 생성자 함수 방식 

```javascript
new RegExp(pattern[, flags]) 
```

-> new RegExp(패턴, 플래그) 함수 문법으로 정규식을 생성할 수 있다. 

```javascript
/front/i;  
new RegExp('front', 'i')
new RegExp(/front/, 'i')
new RegExp(/front/i); 
```

-> 세 개 다 가능하다. 

<br />

>  리터럴과 RegExp 생성자 함수 방식의 **차이점**은 **RegExp 생성자 함수 방식**은 정규식이 실행되는 시점에 컴파일되기 때문에 다른 출처로부터 패턴을 가져오거나 혹은 패턴이 <u>동적으로 변경되어야 하는 경우에 유용</u>하고 **리터럴 방식**은 스크립트 호출 시점에 컴파일되며, <u>고정적인 정규식을 사용할 때 유용</u>하다. 

```javascript
let pattern = 'front'; 

const regExp = new RegExp(pattern, 'i'); 
```

<br />

## 2. 정규표현식 문법 

#### 1) RegExp 메서드 

메서드의 종류로는 RegExp.prototype.**exec**, RegExp.prototype.**test**, RegExp.prototype.**match** , RegExp.prototype.**replace**, RegExp.prototype.**search**, RegExp.prototype.**split** 등이 있다. 

(1) RegExp.prototype.**exec**  

- 패턴을 검색해서 매칭된 결과를 배열로 반환. 매칭 결과가 없는 경우 null 반환
- g 플래그를 지정해도 첫 번째 매칭 결과만 반환함

(2) RegExp.prototype.**test** 

- 패턴을 검색해서 매칭된 결과를 Boolean 값(true, false) 으로 반환 

(3) String.prototype.**match**

- 패턴을 검색해서 매칭된 결과를 배열로 반환. 매칭 결과가 없는 경우 null 반환 
- exec 와의 차이점은 g 플래그를 지정하면 모든 매칭 결과를 배열로 반환  (index, input, groups 프로퍼티는 빠지게 됨)
- 모든 매칭값 정보를 반환하는  matchAll() 메서드도 최근 지원됨 - g 플래그를 반드시 사용해야 함 (아니면 TypeError), index, input, groups 프로퍼티가 있음 

(4) String.prototype.**replace** 

- 매칭되는 부분을 대체 문자열로 치환함
- g 플래그를 지정하면 모든 매칭값을 치환 가능 (replaceAll() 메서드 최근 지원됨) 

(5) String.prototype.**search** 

- 문자열에서 일치하는 부분을 탐색해 일치하는 부분의 index 반환, 없으면 -1 반환 

(6) String.prototype.**split**

- 매칭되는 부분들을 기준으로 문자열을 나누어 배열로 반환

##### <br /> <활용 예시>  

```javascript
const target = 'Front princesses are front developers';
const regExp1 = /front/gi; 
const regExp2 = /back/; 
const regExp3 = /front/i; 

// 1. RegExp.prototype.**exec**   
console.log(regExp1.exec(target));   // ["Front", index:0, input: "Front princesses are front developers", groups: undefined] 
console.log(regExp2.exec(target));   // null 

// 2. RegExp.prototype.**test**    
console.log(regExp1.test(target));   // true    
console.log(regExp2.test(target));   // false 

// 3. String.prototype.**match**  
console.log(target.match(regExp3));  // ["Front", index:0, input: "Front princesses are front developers", groups: undefined]  
console.log(target.match(regExp1));  // ["Front", "front"] 
// matchAll 
const allThing = [...target.matchAll(regExp1)]; 
console.log(allThing); // [["Front", index:0, input: "Front princesses are front developers", groups: undefined], ["front", index:21, input: "Front princesses are front developers", groups: undefined]] 

// 4. String.prototype.**replace**  - replace(pattern, replacement)
console.log(target.replace(regExp1, 'back'));  // "back princesses are back developers"
console.log(target.replace(regExp3, 'back'));  // "back princesses are front developers" 
console.log(target.replaceAll(regExp1, 'back'));  // "back princesses are back developers"  

// 5. String.prototype.**search**    
console.log(target.search(regExp1)); // 0 
console.log(target.search(regExp2)); // -1

// 6. String.prototype.**split** 
console.log(target.split(regExp1)); // ["", " princesses are ", " developers"]
console.log(target.split(regExp2)); // ["Front princesses are front developers"]
console.log(target.split(regExp3)); // ["", " princesses are ", " developers"]
```

<br />

#### 2) 플래그의 종류  

- 플래그는 패턴처럼 필수사항은 아니지만, 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 

| 플래그 | 의미        | 설명                                 |
| ------ | ----------- | ------------------------------------ |
| **i**  | Ignore case | 대소문자 구별 없이 검색              |
| **g**  | Global      | 문자열에 모든 패턴을 검색(전역 검색) |
| m      | Multi line  | 문자열의 행이 바뀌어도 계속 검색     |
| u      | Unicode     | 유니코드 전체를 지원                 |
| y      | sticky      | 문자 내 특정 위치에서 검색을 진행    |
| s      | . 고도화    | .이 개행 문자 \n 도 포함하도록 함    |

-> 하나 이상의 플래그를 동시에 설정할 수도 있다. 플래그를 아예 설정하지 않은 경우에는 대소문자를 구별해서 패턴을 검색하며 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다. 

```javascript
const target = 'Front princesses are front developers. We are good at front development.';

// 대소문자 구별하지 않고 한 번만 검색 
console.log(target.match(/front/i)); // ["Front", index:0, input: "Front princesses are front developers", groups: undefined] 

// 대소문자 구별하여 전역 검색
console.log(target.match(/front/g)); // ["front", "front"] 

// 대소문자 구별하지 않고 전역 검색 
console.log(target.match(/front/gi)); // ["Front", "front", "front"] 
```

<br />

#### 3) 패턴  

- 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.  

- 정규 표현식에서 사용되는 기호를 Meta 문자라고 표현한다. 

| 표현식      | 의미                                                         |
| ----------- | ------------------------------------------------------------ |
| **^x**      | 문자열의 <u>시작을 표현</u>하며 x 문자로 시작됨을 의미한다.  |
| **x$**      | 문자열의 <u>종료를 표현</u>하며 x 문자로 종료됨을 의미한다.  |
| .x          | 임의의 한 문자의 자리수를 표현하며 문자열이 x로 끝난다는 것을 의미한다. |
| **x+**      | 반복을 표현하며 <u>x 문자가 한 번 이상 반복됨</u>을 의미한다. ex. /(Good)+day/g 이면 Goodday, GoodGoodday, GoodGoodGoodday.. 다 가능 |
| **x?**      | 존재여부를 표현하며 x 문자가 <u>존재할 수도, 존재하지 않을 수도 있음</u>을 의미한다. ex. /(Good)?day/g 이면 day, Goodday 가능 |
| **x***      | 반복여부를 표현하며  x 문자가 <u>0번 또는 그 이상 반복됨</u>을 의미한다. ex. /(Good)*day/g 이면 day, Goodday, GoodGoodday .. 다 가능 |
| **x\|y**    | <u>or 을 의미</u>하며 x 또는 y 문자가 존재함을 의미한다.     |
| (x)         | 그룹화 및 캡쳐                                               |
| (x)(y)      | 그룹들의 집합을 표현하며 앞에서부터 순서대로 번호를 부여하여 관리하고 x, y 는 각 그룹의 데이터로 관리된다. |
| (?:x)       | 그룹화 (캡쳐 X)                                              |
| **x{n, m}** | 반복을 표현하며 <u>x 문자가 최소 n번 이상 최대 m번 이하로 반복됨</u>을 의미한다. |
| x{n}        | 반복을 표현하며 x 문자가 n번 반복됨을 의미한다. 즉 {n, n} 인 것임. |
| x{n,}       | 반복을 표현하며 x 문자가 n번 이상 반복됨을 의미한다.         |

그룹화가 이해가 안돼서 예시를 살펴보자 .. 

```javascript
console.log('chaechae'.match(/chae+/)); // "chae" 
console.log('chaeeeeechaeee'.match(/chae+/)); // "chaeeeee"  

// 그룹화를 하지 않으면 이렇게 마지막 글자만 +가 적용이 된다. 

console.log('chaechae'.match(/(chae)+/)); // "chaechae", "chae" 
console.log('chaeeeechaeeee'.match(/(chae)+/)); // "chae", "chae" 
```

오잉 근데 왜 g 플래그 (전역 검색) 를 적용하지 않았는데 2개가 검색이 됐지..?  그건 캡쳐 기능 때문이다. () 안에 있는 "chae" 를 그룹화하여 캡쳐(복사) 한 뒤 먼저 1회 이상 연속으로 반복되는 문자로 검색을 한 뒤 다른 표현식이 모두 작동하고 난 뒤에 캡쳐했던 표현식 "chae"가 검색되는 것이기 때문이다. 

```javascript
console.log('chaechae'.match(/(?:chae)+/)); // "chaechae"
console.log('chaeeeechaeeee'.match(/(?:chae)+/)); // "chae"
```

그렇기 때문에 캡쳐를 하지 않으면 하나만 반환하게 된다. 

<br />

- Meta 문자들 중 좀 더 특수하게 사용되는 문자들이 존재한다. 

| 표현식    | 의미                                                         |
| --------- | ------------------------------------------------------------ |
| **[xy]**  | 문자 선택을 표현하며 x 와 y 중에 하나를 의미한다. [abc] -> a or b or c |
| **[^xy]** | not 을 표현하며 x 및 y 를 제외한 문자를 의미한다.            |
| **[x-z]** | range 를 표현하며 x ~ z 사이의 문자를 의미한다. ex. 0-9 (숫자), a-zA-Z(영어), ㄱ-ㅎ가-힣 (한글), . (모든 문자열을 말함) |
| \^        | escape 를 표현하며 ^ 를 문자로 사용함을 의미한다. 이외에도 특수기호와 사용 가능 ex. \ + *, &, ?, ! ... etc |
| \b        | word boundary 를 표현하며 문자와 공백사이의 문자를 의미한다. |
| \B        | non word boundary 를 표현하며 문자와 공백사이가 아닌 문자를 의미한다. |
| **\d**    | digit 를 표현하며 <u>숫자</u>를 의미한다.                    |
| **\D**    | non digit 을 표현하며 <u>숫자가 아닌 것</u>을 의미한다.      |
| \s        | space 를 표현하며 <u>공백 문자</u>를 의미한다.               |
| \S        | non space 를 표현하며 <u>공백 문자가 아닌 것</u>을 의미한다. |
| \t        | tab 을 표현하며 탭 문자를 의미한다.                          |
| \v        | vertical tab을 표현하며 수직 탭 문자를 의미한다.             |
| **\w**    | word 를 표현하며 <u>알파벳 + 숫자 + _ 중의 한 문자</u>임을 의미한다. [a-zA-Z0-9_] 과 동일 |
| **\W**    | non word 를 표현하며 <u>알파벳 + 숫자 + _ 가 아닌 문자</u>를 의미한다. |

다른 건 아 그렇구나하고 이해가 되는데 \b, \B 는 뭔지 잘 모르겠어서 예시를 살펴보겠다. 

```javascript
const target1 = 'hi, front princess!'; 

console.log(target1.match(/\bfront/g)); 	// ["front"]  
console.log(target1.match(/\Bfront/g));   // null

const target2 = 'hifrontprincess!'; 
console.log(target2.match(/\bfront/g)); // null 	
console.log(target2.match(/\Bfront/g));  // ["front"]  
```

- 쉽게 생각하면 공백 사이에 있으면 (단독 단어 같은 느낌..) \b, 붙어있으면 \B 다. 

<br />

#### 4) 자주 사용하는 정규표현식 

자주 사용하는 정규표현식의 예시를 써볼 건데 .. 이건 사실 그냥 필요할 때마다 찾아보는 형식으로 하는 것이 더 좋을 것 같다!! 굳이 몰라도 된다고 생각..! 

##### (1) 특정 단어로 시작/끝나는지 검사 

```javascript
const url = 'https://naver.com'; 

// http or https 로 시작하는지 검사 
/^https?/.test(url); //true 

// com 으로 끝나는지 검사 
/com$/.test(url); //true 
```

##### (2) 숫자로만 이루어진 문자열인지 검사 

```javascript
const target = '12345'; 

/^\d+$/.test(target) // true 
```

##### (3) 하나 이상의 공백으로 시작하는지 검사 

```javascript
const target = ' Very Hungry!'; 

// \s 는 여러 가지 공백 문자를 의미 
/^[\s]+/.test(target); //true 
```

##### (4) 아이디로 사용 가능한지 검사 

- 예를 들면 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4-10자리인지 검사하기 위해서는 

```javascript
const id = 'abc12345674234324'; 

/^[A-Za-z0-9]{4,10}$/.test(id) // true 
```

##### (5) 메일 주소 형식에 맞는지 검사 

```javascript
const email = 'chaedev3@gmail.com'; 

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email);   //true
```

##### (6)  핸드폰 번호 형식에 맞는지 검사 

```javascript
const phonenumber = '010-0000-0000'; 

/^\d{3}-\d{3, 4}-\d{4}$/.test(phonenumber); //true 	
```

##### (7) 특수 문자 포함 여부 검사 

```javascript
const target = 'aaa##';

(/[^A-Za-z0-9]/gi).test(target)  // true 		
```

<br />

물론 이렇게 써놓긴 했지만, 굉장히 어려운 건 사실이다 (너만 어렵다 패스,,) 저걸 매일 쓰지 않는 이상 계속 기억하고 있기는 쉽지 않을 듯 하니 대충 개념만 알고 있고 필요할 때마다 찾아서 보면 될 것 같고 정규 표현식을 테스트 해볼수있는 사이트가 많으니 찾아서 하면 될 것 같다.. !! 

<br />

##### 참고

- 모던 자바 스크립트 Deep Dive 
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Regular_expressions 
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/search

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split 

- https://hamait.tistory.com/342 

- https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%A0%95%EA%B7%9C%EC%8B%9D-RegExp-%EB%88%84%EA%B5%AC%EB%82%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%BD%EA%B2%8C-%EC%A0%95%EB%A6%AC

- https://bluewings.github.io/unobstructed-hangul-regular-expression/ 

- https://sophiecial.tistory.com/30

