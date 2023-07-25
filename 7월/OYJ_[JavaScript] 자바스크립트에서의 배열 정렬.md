# 자바스크립트에서의 배열 정렬

자바스크립트로 배열을 정렬하는 방법은 `sort()`함수와 `sorted()`함수를 사용하는 것이 있다!

오늘은 `sort()`함수와 `sorted()`함수에 대해 자-세히 알아보려한다.
그럼 들어가기 앞서, 이 두 함수를 주제로 하게 된 계기(= 간단한 퀴즈)로 시작해보려한다~!

``` javascript
// 아래 자바스크립트 코드의 실행 결과는?
[12, 1080, 113, 1].sort();

// 1. [1, 12, 113, 1080]
// 2. [1080, 113, 12, 1]
// 3. [1, 1080, 113, 12]
// 4. [12, 1080, 113, 1]
```

ㄷㄱㄷㄱ.. 정답은? [요기](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Eo_circle_blue_white_number-3.svg/2048px-Eo_circle_blue_white_number-3.svg.png)

어떤가요? 조금이라도 고민을 했거나 정답을 맞추지 못했다면, 
오늘 이 글을 통해서 자바스크립트에서의 배열 정렬을 뽀샤봅시다!!

# sort()
```javascript
const members = ['PDS', 'KKY', 'JCL', 'SYJ', 'OYJ'];
console.log(members.sort());

// console: ['JCL', 'KKY', 'OYJ", 'PDS', 'SYJ']
```

### `sort()`의 특징
1. `sort()` 함수는  배열의 요소를 적절한 위치에 **정렬한 후 그 배열을 반환**한다. 
```javascript
const members = ['PDS', 'KKY', 'JCL', 'SYJ', 'OYJ'];
const sortedMembers = members.sort();

console.log(members); // ['JCL', 'KKY', 'OYJ", 'PDS', 'SYJ']
console.log(sortedMembers); // ['JCL', 'KKY', 'OYJ", 'PDS', 'SYJ']
console.log(members === sortedMembers); // true
```
`sort()`함수를 호출하면, 배열 내에서 값들이 오름차순으로 정렬이 되고, 정렬된 배열을 반환한다.
그렇기 때문에 `sort()`를 호출한 배열과 `sort()`가 반환한 배열은 동일하다고 볼 수 있다!

2. 기본 정렬 순서는 **문자열의 유니코드 코드 포인트**를 따른다.

```javascript
const numbers = [12, 1080, 113, 1];
console.log(numbers.sort());
// console: [1, 1080, 113, 12]
```
`sort()`함수는 배열의 값을 문자열로 변환하여 정렬을 한다..! 이 무슨 당황스런... !!!
그렇기 때문에, 내가 기대했던 `[1, 12, 113, 1080]` 배열이 아닌, 
문자열 기준으로 오름차순으로 정렬된 `[1, 1080, 113, 12]`가 반환되는 것!

그런 `sort()`함수를 음수를 포함한 배열에서 호출하면 어떻게 되겠는가...!
거진.. 사고력 퀴즈다 !!
```javascript
const numbers = [12, 113, 1, -11, -131, -3, 0];
console.log(numbers.sort());
// console: [-11, -131, -3, 0, 1, 113, 12]
```

이러라고 만든 `sort()`함수가 아니다!!
그렇다면 이제 `sort()`함수를 활용해서 올바르게(내가 기대했던 대로?) 정렬하는 방법을 알아보자!

### sort() 함수의 인자 

> #### 콜백함수 compareFunction
> arr.sort([compareFunction])
> 대소비교를 위한 함수로, 2개의 인자를 받는다.


`sort()`함수는 정렬 기준을 콜백함수로 받는다. 이 부분은 선택요소이지만, 사용을 하지 않게되면 위에 언급된 바와 같이 배열은 문자열을 기준으로 비교하여 정렬이 된다.

```javascript
function compare(a, b) {
  if (a < b) {
    return -1;
  }
  if (a > b) {
    return 1;
  }
  // a === b
  return 0;
}

// 인라인: .sort((a, b) => a - b)
```

- `compareFunction(a, b)`이 0보다 작은 경우
a를 b보다 낮은 색인으로 정렬. 즉, a가 먼저 온다.
- `compareFunction(a, b)`이 0을 반환할 경우
a와 b를 서로에 대해 변경하지 않고 모든 다른 요소에 대해 정렬
- `compareFunction(a, b)`이 0보다 큰 경우
b를 a보다 낮은 인덱스로 정렬


### 숫자 배열 정렬
- 숫자 배열의 오름차순 정렬
```javascript
[12, 113, 1, -11, -131, -3, 0].sort((a, b) => a - b)
// [-131, -11, -3, 0, 1, 12, 113]
```

- 숫자 배열의 내림차순 정렬
```javascript
[12, 113, 1, -11, -131, -3, 0].sort((a, b) => b - a)
// [113, 12, 1, 0, -3, -11, -131]
```

### 객체 배열 정렬
```javascript
const members = [
  { id: 1, initial: "KKY", age: 32 },
  { id: 2, initial: "JCL", age: 33 },
  { id: 3, initial: "OYJ", age: 35 },
  { id: 4, initial: "PDS", age: 31 },
  { id: 5, initial: "SYJ", age: 38 },
];
```

- 숫자 정렬
```javascript
members.sort((a, b) => a.age - b.age)

/**
  {id: 4, initial: "PDS", age: 31}
  {id: 1, initial: "SYJ", age: 32}
  {id: 2, initial: "JCL", age: 33}
  {id: 3, initial: "OYJ", age: 35}
  {id: 5, initial: "KKY", age: 38}
**/
```

- 문자열 정렬 (`localeCompare()` 함수 사용)
```javascript
members.sort((a, b) => a.initial.localeCompare(b.initial))
/**
  {id: 2, initial: "JCL", age: 33}
  {id: 5, initial: "KKY", age: 38}
  {id: 3, initial: "OYJ", age: 35}
  {id: 4, initial: "PDS", age: 31}
  {id: 1, initial: "SYJ", age: 32}
**/
```
> `localeCompare()` 메서드는 참조 문자열이 정렬 순으로 지정된 문자열 앞 혹은 뒤에 오는지 또는 동일한 문자열인지 나타내는 수치를 반환



# toSorted()
```javascript
// 함수 없이 사용
toSorted()

// 화살표 함수
toSorted((a, b) => { /* … */ })

// 비교 함수
toSorted(compareFn)

// 인라인 비교 함수
toSorted(function compareFn(a, b) { /* … */ })
```


### toSorted()의 특징
- `toSorted()`함수는 `sort()`에 대응되는 복사 함수이다. 이 함수는 요소들을 오름차순으로 정렬하여 **새로운 배열을 반환**한다.
```javascript
const members = ['PDS', 'KKY', 'JCL', 'SYJ', 'OYJ'];
const sortedMembers = members.toSorted();

console.log(members); // ["PDS", "KKY", "JCL", "SYJ", "OYJ"]
console.log(sortedMembers); // ["JCL", "KKY", "OYJ", "PDS", "SYJ"]
console.log(members === sortedMembers); // false
```

- 빈 요소는 `undefined`로 간주되고, `undefined`는 배열의 마지막으로 정렬되고, compareFunction은 호출되지 않는다.
```javascript
const numbers = [1, 3, 5, , 9];
console.log(numbers.toSorted());
// console: [1, 3, 5, 9, undefined]
```


사용 방법은 대동소이하지만, 동일한 배열로 예시를 들어보겠다!
### 숫자 배열 정렬
- 숫자 배열의 오름차순 정렬
```javascript
const numbers = [12, 113, 1, -11, -131, -3, 0];
const sortedNumbers = numbers.toSorted((a, b) => a - b);
console.log(sortedNumbers);
// console: [-131, -11, -3, 0, 1, 12, 113]
```

- 숫자 배열의 내림차순 정렬
```javascript
const numbers = [12, 113, 1, -11, -131, -3, 0];
const sortedNumbers = numbers.toSorted((a, b) => b - a);
console.log(sortedNumbers);// [113, 12, 1, 0, -3, -11, -131]
```

### 객체 배열 정렬
```javascript
const members = [
  { id: 1, initial: "KKY", age: 32 },
  { id: 2, initial: "JCL", age: 33 },
  { id: 3, initial: "OYJ", age: 35 },
  { id: 4, initial: "PDS", age: 31 },
  { id: 5, initial: "SYJ", age: 38 },
];
```

- 숫자 정렬
```javascript
const sortedMembers = members.toSorted((a, b) => a.age - b.age)
console.log(sortedMembers)
/**
console: 
  {id: 4, initial: "PDS", age: 31}
  {id: 1, initial: "SYJ", age: 32}
  {id: 2, initial: "JCL", age: 33}
  {id: 3, initial: "OYJ", age: 35}
  {id: 5, initial: "KKY", age: 38}
**/
```

- 문자열 정렬 (`localeCompare()` 함수 사용)
```javascript
const sortedMembers = members.toSorted((a, b) => a.initial.localeCompare(b.initial));
console.log(sortedMembers)
/**
console:
  {id: 2, initial: "JCL", age: 33}
  {id: 5, initial: "KKY", age: 38}
  {id: 3, initial: "OYJ", age: 35}
  {id: 4, initial: "PDS", age: 31}
  {id: 1, initial: "SYJ", age: 32}
**/
```

### sort()와 toSorted()함수 추가 예시
- `toSorted()` 배열 바로 호출하기
```javascript
const arrayLike = {
  length: 3,
  unrelated: "foo",
  0: 5,
  2: 4,
};
console.log(Array.prototype.toSorted.call(arrayLike));
// console:  [4, 5, undefined]
```
- `sort()`로 원본 배열을 그대로 두고 복사된 배열 정렬하기 (=`toSorted()`)
```javascript
const words = ['def', 'xyz', 'abc'];
const sortedWords = [...words].sort();
console.log(words); // ["def", "xyz", "abc"]
console.log(sortedWords); // ["abc", "def", "xyz"]
```


# ✌️두 줄 요약
- `sort()`함수와 `toSorted()`함수를 사용할 때에는 콜백함수`compareFunction()`가 없으면 문자열을 기준으로 정렬이 된다는 점을 주의해야한다.
- `sort()`는 해당 배열을 정렬하여 반환, `toSorted()`는 해당 배열을 복사하여 정렬된 새로운 배열을 반환한다.




_참고_
> [mozilla - Array.prototype.sort()
> ](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)[mozilla - Array.prototype.toSorted()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted)
> [DaleSeo님의 블로그](https://www.daleseo.com/js-sort-to-sorted/)