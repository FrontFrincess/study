분명 웹소켓을 사용하긴 했는데, 아직 모르는게 많다. 바르게 사용했는지, 왜 사용했었는지, 더 나은 방법은 없었는지에 대해 정리하기위해 웹소켓에 대해 알아보기로 했다.

### 🔥 웹소켓이란?

 <span style="background-color:#fff5d8;">**데이터 전송 프로토콜** 중 하나로 클라이언트와 서버를 연결하고, 실시간으로 통신이 가능하도록 하는 기술
  </span>
  실시간 알림, 채팅 등 실시간으로 구현해야 하는 기능들을 위해서 웹소켓 기술을 사용한다.
  **TCP 연결**을 통해 **실시간**으로 **양방향 통신**(전이중 통신)을 가능하게 하는 프로토콜이다.

>   TCP가 기억이 나지 않는다면 [TCP 와 UDP 의 차이점](https://velog.io/@eugenieseo16/CS8) 게시물을,
> 전송 계층이 기억나지 않는다면 [OSI 7계층](https://velog.io/@eugenieseo16/CS7) 게시물을
> 다시 읽고 오도록 하자!

- TCP 기반의 프로토콜이기 때문에 **Handshake 과정**을 거친다. 
  TCP layer에서 연결을 하는 다른 프로토콜과는 달리,** HTTP 요청 기반**으로 Handshake 과정을 거쳐 연결한다. Handshake 과정을 통해 통신이 연결되면 응용 프로그램 계층 프로토콜이 HTTP에서 웹 소켓으로 업그레이드가 된다. 이 후, HTTP는 사용되지 않고, 웹 소켓 연결이 닫힐 때까지 두 끝 점에서 웹 소켓 프로토콜을 사용하여 데이터를 주고 받게 된다.

- <span style="background-color:#f4d9d7;"> HTTP 요청으로 시작되어 HTTP에서 동작하지만, 둘은 다르다.
  
  </span>
  HTTP는 Connectionless(비연결성)과 무상태(stateless) 특징을 가지기 때문에 클라이언트와 서버간 접속을 유지하지 않으며, 단방향 통신만 가능하고, 응답 이후에는 연결이 끊긴다. 즉,사용자는 서버로부터 새로운 정보를 받아보기 위해서, 반드시 새로운 URL을 요청해야 한다. 
  **웹 소켓**은 **클라이언트와 서버간 접속이 유지**되며 (클라이언트와 서버 간의 연결은 당사자 중 하나에 의해 종료되거나 시간 초과에 의해 닫힐 때까지 열린 상태로 유지된다) 요청과 응답 개념이 아닌 서로 **데이터를 주고 받는 형식**이다. 
  REST한 방식의 HTTP 통신에서는 많은 URI와 Http Method를 통해 웹 어플리케이션과 상호작용하지만, 웹 소켓은 초기 연결 수립을 위한 오직 하나의 URL만 존재하며, 모든 메시지는 초기에 연결된 TCP 연결로만 통신한다.

- **HTML5와 호환**되며 이전 HTML 버전과의 **역호환성**을 제공한다.
  모든 최신 웹 브라우저에서 지원될 뿐만 아니라  Android, iOS, 웹 앱 및 데스크톱 앱과 같은 플랫폼 간에도 호환된다. 단일 서버는 여러 WebSocket 연결을 동시에 열 수 있고, 동일한 클라이언트에 대한 여러 연결을 열어 쉽게 확장이 가능하다.
  
  > **역호환성** :기술 및 컴퓨터 분야에서 새 제품이 이전 제품을 염두에 두고 만들어진 제품에서 별도의 수정 없이 그대로 쓰일 수 있는 것

커넥션 중단과 추가 HTTP 요청 없이 양방향으로 이루어지기 때문에 온라인 게임이나 주식 트레이딩 시스템같이 데이터 교환이 지속적으로 이뤄져야 하는 서비스에 적합하다.

- 실시간 구현을 위해 HTTP에서도 다양한 노력을 했다.
  **HTTP 폴링 (Polling), HTTP 롱폴링 (Long-Polling), HTTP Streaming**
  하지만 클라이언트의 요청을 통해야만 서버에게 데이터를 전송 받는다는 한계점이 존재하고, 불필요한 헤더와 높은 트래픽을 야기한다는 단점이 있어 실시간 구현에 사용하기 적합하지 않다.

![](https://velog.velcdn.com/images/eugenieseo16/post/0a0626da-609b-46db-a157-7686c636388c/image.png)

#### ✅ 웹소켓 사용 예시

웹소켓을 연결하려면 new WebSocket을 호출하면 된다. ws라는 특수 프로토콜을 사용한다.

```javascript
const ws = new WebSocket('wss://eugenieseo16/ws/chat');
```

> **ws** : 일반 webSocket
> **wss** : SSL이 적용된 webSocket
> HTTP와 HTTPS의 관계와 유사하다. wss는 보안, 신뢰성 측면에서 ws 보다 좀 더 신뢰할만한 프로토콜이다.

- 사용할 수 있는 이벤트
  ![](https://velog.velcdn.com/images/eugenieseo16/post/6d1cd0af-268e-4bef-ab3d-39c98df183bf/image.png)
  <span style="background-color:#d0edec;">

- **open **: 통신이 시작되었을 때
  
  </span>
  ```javascript
    ws.onopen = () => {
      console.log('WebSocket connected');
  
      // 페이지에 처음 접속했을 때 환영 문구 보내기
      const welcomeMessage: Message = {
        content: '환영합니다!,
        timestamp: Date.now(),
      };
      setMessages(prevMessages => [...prevMessages, welcomeMessage]);
  
    };
  
  ```
  <span style="background-color:#d0edec;">
  ws.onopen
  </span>
  을 사용하여, 통신이 연결되었을 때 (=사용자가 페이지에 접속했을 때) 환영 메시지를 보낼 수 있다.
  ```

- 한번 연결이 되면 WebSocket 오브젝트의 send()를 사용하여 서버로 데이터를 전송할 수 있다. 

```javascript
    // WebSocket을 통해 메시지 전송
    socket?.send(JSON.stringify(message));
```

- **message**  :  데이터를 수신하였을 때
  
  ```javascript
    // 메시지가 오면 목록에 추가
    ws.onmessage = evt => {
      const message: Message = JSON.parse(evt.data);
      setMessages(prevMessages => [...prevMessages, message]);
    };
  ```

```
- ** error **: 에러가 생겼을 때
```javascript
    ws.onerror = error => {
      console.log(error);
    };
```

- ** close **: 통신이 종료되었을 때
  웹 소켓 사용을 마쳤다면 close() 메소드를 호출해 연결을 종료한다.
  
  ```javascript
    ws.close();
  ```

한쪽에서 통신 종료를 원하는 경우에는 보통 숫자코드와 이유가 담긴 '커넥션 종료 프레임’을 전송한다.

```javascript
    ws.close([code], [reason]);
```

**code** – 통신을 종료할 때, 사용하는 특수 코드(옵션)
**reason** – 통신 종료 이유를 설명하는 문자열(옵션)

다음과 같이 코드와 사유를 확인할 수 있다.

주체:

```
socket.close(1000, "Work complete");
```

통신 상대방 :

```
ws.onclose = event => {
  event.code === 1000
  event.reason === "작업 완료"
};
```

1000 – 기본값. 정상 종료를 의미
1001 – 연결 주체 중 한쪽이 떠남
1009 – 메시지가 너무 커서 처리하지 못함
1011 – 서버 측에서 비정상적인 에러 발생
이 외의 코드는 [여기서](https://datatracker.ietf.org/doc/html/rfc6455#section-7.4.1) 확인이 가능하다. 

#### ✅ Socket.io

자바스크립트를 이용하여 브라우저 종류에 상관없이 실시간 웹을 구현할 수 있도록 한 기술이다.

웹소켓은 HTML5의 기술이기 때문에 오래된 버전의 웹 브라우저는 웹소켓을 지원하지 않을 수 있다. 이를 해결하기 위해 나온 기술 중 하나가 Socket.io이다. 

웹소켓을 지원하는 브라우저의 경우, 웹소켓 방식으로 동작하고, 지원하지 않는 브라우저의 경우, 일반 http를 이용해서 실시간 통신을 흉내내는 방식을 사용한다.
node.js 기반으로 만들어진 기술로 거의 모든 웹 브라우저와 모바일 장치를 지원하는 실시간 웹 애플리케이션 지원 라이브러리이다.

> 참고 : 
> https://datatracker.ietf.org/doc/html/rfc6455#section-7.4.1
> https://ko.javascript.info/websocket
> https://tecoble.techcourse.co.kr/post/2021-08-14-web-socket/
> https://developer.mozilla.org/ko/docs/Web/API/WebSockets_API/Writing_WebSocket_client_applications
> https://velog.io/@rhdmstj17
> https://appmaster.io/ko/blog/websocketiran-mueosimyeo-eoddeohge-saengseonghabnigga
> https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-%EC%9B%B9-%EC%86%8C%EC%BC%93-Socket-%EC%97%AD%EC%82%AC%EB%B6%80%ED%84%B0-%EC%A0%95%EB%A6%AC
