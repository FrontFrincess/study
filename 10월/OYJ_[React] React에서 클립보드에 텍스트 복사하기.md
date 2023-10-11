# React에서 클립보드에 텍스트 복사하기

![post-thumbnail](https://velog.velcdn.com/images/yjohbjects/post/5e053a72-021d-4d17-846b-343e5eef3617/image.png)

`클립보드로 복사하기` 기능은 웹사이트에서 자주 볼 수 있는 편리한 기능이다.
react로 이 기능을 구현할 수 있는 방법은 여러가지가 있는데, 하나씩 보도록하자!

# 1. document.execCommand() - 지원중단


![](https://velog.velcdn.com/images/yjohbjects/post/334a91d6-28c8-451a-91a2-234e96a6c89f/image.png)

우선 이 방법은 공식문서에서 볼 수 있다시피, 지원이 중단되었다. 
이 글을 작성하는 시점에서 작동은 되지만, 공식문서를 기준으로한다면 사용하지 않는 것이 맞겠다.
하지만 다른 방법들과 비교를 위해서, document.execCommand()의 사용방법도 알아는?보자!

## 사용방법
document.execCommand()함수에서 우리가 사용할 매개변수는 `copy`, `cut`, `paste`이다.
- `document.execCommand("copy")`: 클립보드에 복사하기
- `document.execCommand("cut")`: 클립보드에 잘라내기
- `document.execCommand("paste")`: 클립보드의 내용 붙여넣기

## 코드
```tsx
function App() {
  const handleCopyClipBoard = (text: string) => {
    const $textarea = document.createElement("textarea"); // 임시요소 생성해서 부착하고
    document.body.appendChild($textarea);
    $textarea.value = text;
    $textarea.select(); // 선택해서
    document.execCommand("copy"); // 복사하고
    document.body.removeChild($textarea); // 임시 요소 제거까지
    alert("계좌번호가 복사되었습니다.");
  };

  
  return (
     <div>
 	   <p>신한은행 xxx-xxx-xxxxxx</p>
          <button
            onClick={() => handleCopyClipBoard("신한은행 xxx-xxx-xxxxxx")}
          >
            계좌복사
          </button>
	</div>
  );
```

하지만 리액트에서도 다시한번 알려주는 현실은 **지원 중단** 됐다는 사실!
그럼 빠르게 다음 방법으로 넘어가자.
![](https://velog.velcdn.com/images/yjohbjects/post/365e120f-192d-406a-b079-2863896939e3/image.png)


# 2. Clipboard API

Clipboard API는 `navigator` 객체가 제공한다.
> **`navigator` 객체**
> navigator 객체는 브라우저 공급자 및 버전 정보 등을 포함한 브라우저에 대한 다양한 정보를 저장하는 객체로, window객체와 같이 전역으로 가지고 있다.

아래 두가지 사항만 유의하여 사용하면, 큰 문제는 없을 방법이다!
 - IE 미지원
 - Safari 13.1부터 https 환경에서만 지원

## 사용방법
우리는 `navigator` 객체 중에서도 `navigator.clipboard`의 아래 함수들을 사용해서 클립보드에 복사를 할 것이다.
- `navigator.clipboard.writeText()`: 클립보드에 복사하기
- `navigator.clipboard.readText()`: 클립보드의 내용 붙여넣기

## 코드
``` tsx
function App() {
  const handleCopyClipBoard = async (text: string) => {
    try {
      await navigator.clipboard.writeText(text);
      alert("copied");
    } catch (e) {
      alert("failed");
    }
  };
  
  return (
     <div>
 	   <p>신한은행 xxx-xxx-xxxxxx</p>
          <button
            onClick={() => handleCopyClipBoard("신한은행 xxx-xxx-xxxxxx")}
          >
            계좌복사
          </button>
	</div>
  );
```


# 3. react-copy-to-clipboard
마지막으로 소개할 방법은 react-copy-to-clipboard라는 라이브러리다.
아마 소개한 방법들 중 가장 간단한 방법이 될 것이다.

## 사용 방법
`<CopyToClipboard>` `</CopyToClipboard>` 컴포넌트 안에는 1개의 Child만 넣을 수 있는데, 이 Child를 클릭하면 클립보드에 텍스트가 복사된다.

그렇다면 사용방법으로 고고!

### 설치
``` bash
npm install --save react-copy-to-clipboard
```

### 사용
```tsx
import { CopyToClipboard } from "react-copy-to-clipboard";

function App() {
  
  return (
     <div>
      <p>카카오뱅크 xxxx-xx-xxxx-xxx</p>
      
      <CopyToClipboard
        text="카카오뱅크 xxxx-xx-xxxx-xxx"
        onCopy={() => alert("계좌번호가 복사되었습니다.")}
      >
        <button>계좌복사</button>
      </CopyToClipboard>
	</div>
  );
```


> **Source**
> https://developer.mozilla.org/ko/docs/Web/API/Document/execCommand
> https://developer.mozilla.org/ko/docs/Web/API/Clipboard_API