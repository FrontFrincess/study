![](https://velog.velcdn.com/images/somda/post/5153016e-42fd-45b7-911a-1b83dae495ec/image.gif)

강아지를 무한대로 즐길 수 있게 무한 스크롤을 구현해보자.

## 1. 페이지네이션과 무한스크롤
 페이지네이션으로 2페이지, 3페이지 이렇게 넘어갈 수 있음에도 굳이 무한 스크롤을 쓰는 이유는 뭘까? 클릭 하나도 결국 수고로움이기에 그런 것들을 줄이기 위한 고민으로 나오지 않았을까 싶다. 
 물론 모든 웹사이트에서 무한 스크롤이 유저 경험에 낫다는 걸 말하는 것은 아니다.

 예를 들어 인스타그램 피드와 같은 SNS는 훑으며 지나가는, 구경하는 서비스이기에 무한 스크롤이 적합할 것이고 무신사와 같은 쇼핑몰은 상품의 위치를 기억하고 언제든 다시 그 상품을 눌러보는 것이 중요하기 때문에 페이지네이션이 적합할 것이다.

 그렇다면 결국 적재적소에 활용하기 위해서는 둘 다 알아야하지 않겠는가 😎


## 2. 무한 스크롤, 어떻게 구현할까?

### Scroll Event
처음에는 scroll 이벤트를 감지하다가, 페이지 가장 아래에 닿았을 때 쯤 API 요청을 하면 되지 않을까? 싶었다.
하지만 이렇게 구현하게 되면 scroll 이벤트를 계~속 감지해야하기 때문에 성능 저하로 이어질 수 있는 문제가 있다.

이를 해결하기 위해 `debounce`나 `throttle`를 적용해 일부 성능을 개선할 수 있다. 두가지 모두 시간의 텀을 두어 조금이나마 이벤트 감지 횟수를 줄일 수 있게 한다. 
두 가지 모두 성능 개선에 그래도 꽤 도움을 주는 방법들이라 추후에 정리하기로 하고... 

### Intersection Observer API
우선은 Intersection Observer API를 사용하는 방법을 택하기로 한다.

> Intersection Observer API는 타겟 요소와 상위 요소 또는 최상위 document 의 viewport 사이의 intersection 내의 변화를 비동기적으로 관찰하는 방법입니다. (출처: MDN)

해당 API를 통해 스크롤을 내리다가 타겟이 viewport에 들어오면 데이터를 호출하는 식으로 구현할 수 있다.


## 4. 그럼 구현해보자!

### 어떤 걸로?
귀여운 강아지들을 무한대로 즐겨볼까 싶어 dogAPI를 활용했다. 🐶
dogAPI 사용법은 [이곳](https://documenter.getpostman.com/view/5578104/2s935hRnak)에서 확인할 수 있다.

우선 dogAPI를 사용해보자.
params도 API URL에 한꺼번에 넣어서 냅다 요청을 보내본다.
```typescript
  useEffect(() => {
    console.log("로드");

    const API_URL =
      "https://api.thedogapi.com/v1/images/search?size=small&format=json&has_breeds=true&order=ASC&page=0&limit=10";
    axios.get(API_URL).then((res) => {
      console.log(res);
    });
  }, []);

  return (
    <div className="dog-imgs-container">
      <p>cute</p>
    </div>
  );
}
```

데이터는 이렇게 id, url정보와 원본 이미지의 width와 height 정보까지 반환해준다.
![](https://velog.velcdn.com/images/somda/post/4be9a46a-4d16-4cbb-a4f1-70872ca00b0c/image.png)


그럼 빨리 귀여운 강아지를 봐보자.
```ts
import React, { useEffect, useState } from "react";
import axios from "axios";

interface dogImgInterface {
  id: string;
  dogUrl: string;
}

export default function Dogs() {
  const [dogImgArr, setDogImgArr] = useState<dogImgInterface[]>([]);

  useEffect(() => {
    console.log("로드");

    // key가 없으면 응답은 10개씩
    const API_URL =
      "https://api.thedogapi.com/v1/images/search?size=small&format=json&has_breeds=true&order=ASC&page=0&limit=10";
    axios.get(API_URL).then((res) => {
      console.log(res);
      
      // id값과 url만 저장
      const gotData = res.data.map((dogImg: { id: string; url: string }) => ({
        id: dogImg.id,
        dogUrl: dogImg.url,
      }));
      setDogImgArr(gotData);
    });
  }, []);

  return (
    <div className="dog-imgs-container">
      {dogImgArr &&
        dogImgArr.map((dogImg: dogImgInterface) => (
          <div key={dogImg.id} className="dog-img-card">
            <img src={dogImg.dogUrl} />
            <p>cute_{dogImg.id}</p>
          </div>
        ))}
    </div>
  );
}
```

![](https://velog.velcdn.com/images/somda/post/c8ef7a1e-3db3-422b-8c57-dca91e352735/image.png)
아주 잘 불러와진다!

### 이제 무한 스크롤을 적용해보자!

1. 우선 관찰 대상이 될 대상을 하나 만들고, 페이징을 준비한다.
```ts
    <div className="dog-imgs-container">
      {dogImgArr &&
        dogImgArr.map((dogImg: dogImgInterface) => (
          <div key={dogImg.id} className="dog-img-card">
            <img src={dogImg.dogUrl} />
            <p>cute_{dogImg.id}</p>
          </div>
        ))}
      {isLoading && <p>Loading...</p>}
      <div id="observer" style={{ height: "10px" }}></div>
    </div>
```
페이지 최하단에 id가 observer인 빈 div를 하나 만들었다.
Loading도 표시해봤다.

```ts
  const [page, setPage] = useState(0);
  const [isLoading, setIsLoading] = useState(false);
  const [dogImgArr, setDogImgArr] = useState<dogImgInterface[]>([]);
```
무한스크롤도 따지고 보면 일종의 페이지네이션이기 때문에 페이지 정보와, 로딩 정보도 useState를 활용해 추가로 담아준다.

2. Intersection Observer API를 사용해보자!

인스턴스는
`const observer![](https://velog.velcdn.com/images/somda/post/7f8adfaf-2592-46dd-9242-c23e534c5725/image.gif)
 = new IntersectionObserver(callback, options);`
이와 같이 생성할 수 있다.
`callback` , `options` 2개의 파라미터를 받는데

나는 callback함수로 handleObserver를 만들고, 옵션은 바로 작성했다.
(callback 함수는 파라미터로 `entries`와 `observer` 를 받을 수 있다.)

```ts
  // Intersection Observer 설정

  const handleObserver = (entries: IntersectionObserverEntry[]) => {
    const target = entries[0];
    if (target.isIntersecting && !isLoading) {
      setPage((prevPage) => prevPage + 1);
    }
  };
  /*
  handleObserver: 교차점이 발생했을 때 실행되는 콜백 함수.
  entries: 교차점 정보를 담는 배열
  isIntersecting: 교차점(intersection)이 발생한 요소의 상태
  교차점이 발생하면 page 1 증가
  */

  useEffect(() => {
    const observer = new IntersectionObserver(handleObserver, {
      threshold: 0, //  Intersection Observer의 옵션, 0일 때는 교차점이 한 번만 발생해도 실행, 1은 모든 영역이 교차해야 콜백 함수가 실행.
    });
    // 최하단 요소를 관찰 대상으로 지정함
    const observerTarget = document.getElementById("observer");
    // 관찰 시작
    if (observerTarget) {
      observer.observe(observerTarget);
    }
  }, []);
```

※ 참고: entries가 담고 있는 정보는 다음과 같다.
  ![](https://velog.velcdn.com/images/somda/post/30909702-7dca-4bb3-990a-68f6795620ea/image.png)


위에서 최하단에 지정했던 관찰대상을 만나면 페이지가 1 추가되게 했다.

3. 그럼 이제 페이지가 추가됨에 따라 API를 호출해보자!

```ts
  // page 변경 감지에 따른 API호출
  useEffect(() => {
    fetchData();
    // console.log(page);
  }, [page]);

  // API를 호출하는 부분
  const fetchData = async () => {
    setIsLoading(true);
    try {
      const API_URL = `https://api.thedogapi.com/v1/images/search?size=small&format=json&has_breeds=true&order=ASC&page=${page}&limit=10`;
      const response = await axios.get(API_URL);
      const newData = response.data.map(
        (dogImg: { id: string; url: string }) => ({
          id: dogImg.id,
          dogUrl: dogImg.url,
        })
      );
      // 불러온 데이터를 배열에 추가
      setDogImgArr((prevData) => [...prevData, ...newData]);
    } catch (error) {
      console.log(error);
    }
    setIsLoading(false);
  };
```

4. 완성!

![](https://velog.velcdn.com/images/somda/post/0b9480f9-e996-4631-a0d3-0c5e3b9fa350/image.gif)

슥삭슥삭 잘 넘어간다 :)


### 개선해본다면?
 현재 코드는 타겟이 페이지 최하단에 위치해있어 스크롤이 최하단에 도달해야 API를 호출하기 때문에 인터넷 속도에 따라 로딩이 길어지기도 한다.
 useRef를 활용해 배열의 절반 정도 쯤에 target을 설정하여 조금 더 미리 API를 호출하는 방식도 존재할 것이다. 다만 이렇게 하면 불필요한 시점에 API를 호출하게 될 수도 있어 이것도 어디까지나 상황에 맞게 적용하면 될 것 같다. :)

 이전에 프로젝트를 하면서 무한스크롤을 적용해보지 못해 아쉬웠는데 이렇게나마 적용해보니 참 좋다~ 😎