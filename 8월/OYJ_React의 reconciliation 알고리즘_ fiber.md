# React의 reconciliation 알고리즘: fiber

## Reconciler

Reconciliation은 리액트의 랜더링 로직으로,
리액트 컴포넌트가 화면에 랜더링 되는 과정은 아래와 같다!

1. JSX 파일이 React.createElement로 바벨에 의해 트랜스파일링 된다
2. React.createElement 함수 호출에 의해 리액트 엘리먼트 트리가 반환된다
3. React의 reconciliation 알고리즘에 의해 리액트 엘리먼트 트리를 재귀적으로 순회하면서 이전트리와 현재트리의 변경사항을 비교한 다음 변경된 부분만 실제 DOM에 반영한다

## Reconciliation 알고리즘

Reconciliation 알고리즘은 기존에 stack 이였다가 React 16에서 부터 fiber로 변경되었다
(실직적으로 사용된 것은 React 18에 useTransaction의 등장 부터였지만..)

> fiber architecture가 처음 등장했을 때 소개 영상
> https://www.youtube.com/watch?v=ZCuYPiUIONs&ab_channel=MetaDevelopers

### STACK

가장 나중에 들어간 노드가 가장 먼저 꺼내져야하는 노드 (LIFO)

### FIBER

가장 나중에 들어간 노드를 가장 먼저 꺼낼 필요가 없다
들어간 순서와 관계없이 꺼낼 수 있다

## 그렇다면 왜 Fiber?

리액트에서의 "꺼낸다"의 의미는 DOM에 적용하여 변화를 적용한다는 말이다
stack을 사용하면 꺼내는 순서에서 유연성을 잃는다.
꺼내는 순서를 유연하게 할 수 있는 fiber를 적용!

![](https://velog.velcdn.com/images/yjohbjects/post/278e3666-546e-4315-9d77-cf0464dd9eb9/image.gif)
왼쪽이 기존 stack 아키텍처 vs 오른쪽이 새로운 fiber 아키텍처를 이용했을 때의 react 모습이다

### stack이 부자연스러운 이유에 대해서 자세히 알아보자!

![](https://velog.velcdn.com/images/yjohbjects/post/207587f8-9bf6-47a4-b515-aee232e781dc/image.png)
reconciliation은 elements를 만들고, instance를 생성하고 난 후 DOM에 붙이거나 업데이트를 한다
기존 방식은 재귀적으로 mount를 호출하면서 트리의 최상단 노트까지 가면서 업데이트를 진행한다.

재귀적으로 업데이트가 진행될 시, 중간에 끊을 수 가 없다!! 그래서 사진을 보면 메인쓰레드가 업데이트할 노드를 찾으로 계속 아래로 가는 모습을 볼 수 있다.

예시로, 재귀적으로 업데이트가 진행되고있는데, 애니메이션이 비동기로 동작하고있다면 애니메이션이 제 때 실행되지 못하는 상황이 발생되어서 위의 비교 영상과 같이 애니메이션이 부자연스럽게 찍히는 상황이 발생할 것이다.

쓰레드가 각각의 업데이트를 실행함에도 전체적인 작업을 관리감독하는 것은 어려움이 있을 것이다

여기서 reconciler가 효율적으로 작동하지 못해, 랜더링 과정에서 시간이 많이 소요된다

### 반면 fiber는

![](https://velog.velcdn.com/images/yjohbjects/post/21e25ac4-54ed-41e4-9f9d-78a5b7b9b55a/image.png)
지속적으로 아래로 내려가는 것이 아닌, 업데이트를 해야할 노드가 있다면 마킹을 하고 위로 올라온다
(해당 과정을 그래프로 표현하면 섬유조직의 모습과 비슷하여 fiber라는 이름을 사용한다고...)
다시 말해, 트리의 작은 부분만 계산하고, 다시 위로 올라와서 다음 작업을 살핀다


## React Fiber

fiber는 자체 가상 스택을 사용하는 작업단위로, fiber는 재귀대신 연결리스트를 사용한다
작업이 끝나면 그 다음 작업을 링크타고가서 진행하는 방식

react의 element와 react fiber node는 1:1로 대응되어서
하나의 엘리먼트를 렌더링하는 것은 곧 하나의 작은 단위인 fiber로 맵핑이 되는 것이다


> https://www.youtube.com/watch?v=ZCuYPiUIONs&ab_channel=MetaDevelopers
> https://github.com/acdlite/react-fiber-architecture
> https://www.alibabacloud.com/blog/a-closer-look-at-react-fiber_598138
