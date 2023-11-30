# [React] React Developer Tools



요즘 회사에서 코드 분석을 하는데 도움이 많이 되었던 tool인 'React Developer Tools' 를 소개해보도록 하겠다. 

알고 있는 사람도 많겠지만 암튼 난 몰랐으니까 .. 


리액트 개발 도구를 브라우저 확장 도구로 설치해 리액트 애플리케이션을 디버깅하는 방법을 살펴보고 컴포넌트 렌더링에 대해 이해해보도록 하자! 

### 설치 
설치는 간단하다! 

먼저, 크롬의 확장 프로그램에서 "React Developer Tools" 를 설치한다. 

![](https://velog.velcdn.com/images/chaedev3/post/a059bbdc-9ed2-44ce-98bf-22f5739ab807/image.png)

이렇게 설치가 되었다면, 리액트로 개발된 페이지에 접근을 했을 때 
![](https://velog.velcdn.com/images/chaedev3/post/285706fb-5383-4663-b20f-9fa39ebd4c0a/image.png)

이렇게 뜨게 되고 (리액트로 안 만들어졌다면 disabled로 뜨게 됨!) 
![](https://velog.velcdn.com/images/chaedev3/post/cbb6fc79-4e17-4a83-b2d9-03b2921c0487/image.png)

개발자 도구에 원래 없던 components 와 profiler 탭이 생긴 걸 확인할 수 있다. 

### Component  

> 현재 리액트 애플리케이션의 컴포넌트 트리를 확인할 수 있다. 단순히 컴포넌트의 구조 뿐만 아니라 props 와 내부  hooks 등 다양한 정보를 확인할 수 있다. 


만들다가 만 내 todo list 를 보면서 설명을 하겠다. 
![](https://velog.velcdn.com/images/chaedev3/post/f5e7d56d-ea22-4183-a06d-5371655ed903/image.png)



이렇게 component 탭에서는 컴포넌트 트리 구조를 확인할 수 있다. 

![](https://velog.velcdn.com/images/chaedev3/post/c79a463b-2e8a-4de4-9920-b99f614625b0/image.png)
그리고 이런식으로 왼쪽 오른쪽 둘 다 어디를 선택하던지 어느 컴포넌트를 가르키는 건지를 알 수 있다.

또 개발자 도구와 비슷하게, 값을 바꿔서 확인을 해 보는 것도 가능하다. 

하지만 익명 함수로 작성한 경우에는 Anoymous 로 뜨게 되는데, 이는 리액트 16.8 이후 버전에서는 개선되었다고는 하나 익명 함수로 작성한 경우에는 컴포넌트를 특정하기가 어려워진다는 단점이 있다. 

예를 들어, 리액트 기반인 넷플릭스를 보면 이렇게 중간 중간 Anoymous 가 끼어 있는 것을 알 수 있다. 
![](https://velog.velcdn.com/images/chaedev3/post/91e1948f-306b-411d-9d26-e732726f28ae/image.png)


Component 의 경우에는 사실 작은 규모의 프로젝트는 굳이? 저걸로 보지 않아도 될 것 같은데 복잡한 코드를 분석해야 할 때 어떤 게 무슨 컴포넌트인지 모를 때나 props, hooks 를 보고 싶을 때 쓰면 좋을 것 같다. 


그 다음은 프로파일러이다. 

### Profiler

> 리액트가 렌더링할 때 어떠한 일이 벌어지는 확인할 수 있는 도구다.

![](https://velog.velcdn.com/images/chaedev3/post/d18d8bda-666e-456d-b451-f5e7879ed13a/image.png)

첫 번째 버튼은 프로파일링 시작버튼
두 번째는 새로고침 후 프로파일링 시작버튼
세번째는 프로파일링 종료 버튼으로 프로파일링 된 내용을 지우는 버튼 
네번째 버튼은 프로파일 불러오기 버튼
다섯번째 버튼은 프로파일 저장하기 버튼 - 프로파일링 결과를 저장하면 사용자의 브라우저에 해당 프로파일링 정보가 담긴 JSON 파일이 다운로드됨 

그렇다면, 컴포넌트 렌더링 되는 과정을 profiler 와 함께 알아보자! 

리액트 딥다이브에 나온 코드를 가져와 봤다. 
```React
import { useEffect, useState } from "react";

function App() {
  const [text, setText] = useState("");
  const [number, setNumber] = useState(0);
  const [list, setList] = useState([
    { name: "apple", amount: 5000 },
    { name: "orange", amount: 1000 },
    { name: "watermelon", amount: 3000 },
    { name: "pineapple", amount: 2000 },
  ]);

  useEffect(() => {
    setTimeout(() => {
      console.log("surprise!");
      setNumber("1000");
    }, 3000);
  });

  function handleTextChange(e) {
    setText(e.target.value);
  }

  function handleSubmit() {
    setList((prev) => [...prev, { name: text, amount: number }]);
  }

  function handleNumberChange(e) {
    setNumber(e.target.valueAsNumber);
  }

  return (
    <div>
      <input type="text" value={text} onChange={handleTextChange} />
      <input type="number" value={number} onChange={handleNumberChange} />
      <button onClick={handleSubmit}>추가</button>
      <ul>
        {list.map((value, key) => (
          <li key={key}>
            {value.name} {value.amount}원
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

일단, 컴포넌트 분리의 필요성을 말하려고 App.js 에 다 때려박은 코드이다. 

과일 이름과 가격을 담은 리스트에 이름과, 가격을 입력하고 Add 하는 코드이고, 리스트를 보여주는 코드이다. 

렌더링을 하는 걸 보기 전에 General 탭에 Highlight updates when components redner 옵션을 체크하면 컴포넌트가 렌더링 될 때 마다 강조 표시가 된다.

리렌더링이 되면 파란색 선으로 리렌더링되는 컴포넌트를 표시해주고, 연속적인 리렌더링이 발생하면 노란색으로 표시를 해준다. 

![](https://velog.velcdn.com/images/chaedev3/post/96708e2b-df82-422c-8a92-17077bbfbfef/image.png)


코드는
https://medium.com/wantedjobs/react-profiler%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%84%B1%EB%8A%A5-%EC%B8%A1%EC%A0%95%ED%95%98%EA%B8%B0-5981dfb3d934 
이 블로그에 있는 코드를 사용했다.

```React 
import { useEffect, useState } from "react";
import "./App.css";
import PhotoOne from "./PhotoOne";
import PhotoTwo from "./PhotoTwo";

function App() {
  const [message, setMessage] = useState("");
  const [photos, setPhotos] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/photos")
      .then((response) => response.json())
      .then(setPhotos);
  }, [setPhotos]);

  return (
    <div>
      <input
        value={message}
        onChange={(event) => setMessage(event.target.value)}
      />
      <div className="photos">
        <PhotoOne photos={photos} message={message} />
        <PhotoTwo photos={photos} message={message} />
      </div>
    </div>
  );
}

export default App;

```
설명을 하자면, openAPI 를 통해 photos 를 불러온다.

그것과는 별개로 message 를 입력을 하면 그것을 PhotoOne 과 PhotoTwo에 각각 띄워주는 코드이다. 

비교를 위해 PhotoOne 은 하나로 묶고 PhotoTwo는 컴포넌트를 나눠주었다. 

```React 
PhotoOne.js 
const PhotoOne = ({ message = "", photos = [] }) => {
  return (
    <div>
      <h1>PhotoOne</h1>
      <p>{message}</p>
      <ul>
        {photos.map(photo => {
          return (
            <li key={photo.id}>
              <img src={photo.url} alt={photo.title} />
            </li>
          );
        })}
      </ul>
    </div>
  );
};

export default PhotoOne;
```

```React 
PhotoTwo.js 
const Message = ({ message }) => {
  return <p>{message}</p>;
};

const ListItem = ({ photo }) => {
  return (
    <li key={photo.id}>
      <img src={photo.url} alt={photo.title} />
    </li>
  );
};

const List = ({ photos }) => {
  return (
    <ul>
      {photos.map(photo => (
        <ListItem key={photo.id} photo={photo} />
      ))}
    </ul>
  );
};

const PhotoTwo = ({ message = "", photos = [] }) => {
  return (
    <div>
      <h1>PhotoTwo</h1>
      <Message message={message} />
      <List photos={photos} />
    </div>
  );
};

export default PhotoTwo;
```



'채린'이라는 글자를 썼을 때

![](https://velog.velcdn.com/images/chaedev3/post/406b7598-60ee-44d2-b9da-1d2fd55b27d3/image.png)
![](https://velog.velcdn.com/images/chaedev3/post/45b52474-090c-43bf-917e-e1860cb1a244/image.png)




아니 메세지만 업데이트했는데 전체가 렌더링 되잖아효!! 나는 메세지 컴포넌트만 재렌더링 하고 싶다구용~~ 

이럴 때 React.memo을 이용해보자 
PhotoOne 과 PhotoTwo 모두 React.memo 를 이용했을 때
![](https://velog.velcdn.com/images/chaedev3/post/32c909b0-d906-4c0d-a649-6fcddcf13724/image.png)
![](https://velog.velcdn.com/images/chaedev3/post/55a23d0f-63f8-4d1f-9526-048db92139ff/image.png)


성능이 향상된 걸 알 수 있다!! 아직 어떤 식으로 활용해야 할 지는 모르겠지만 (?) React 동작 방식 이해나 성능 향상에 이용하면 좋을 것 같다! 

### 참고
- 모던 리액트 딥다이브 
- https://m.blog.naver.com/pjt3591oo/221907792621  
- https://medium.com/wantedjobs/react-profiler%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-%EC%84%B1%EB%8A%A5-%EC%B8%A1%EC%A0%95%ED%95%98%EA%B8%B0-5981dfb3d934