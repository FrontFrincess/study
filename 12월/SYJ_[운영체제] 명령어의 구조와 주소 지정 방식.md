명령어는 어떻게 구성되어 있으며, 컴퓨터는 어떻게 명령어를 이해하고, 실행하는 걸까?

고급 프로그래밍 언어로 작성된 코드가 실행되기 위해서는, 컴퓨터가 이해할 수 있는 저급 언어로 변환되어야한다. 
![](https://velog.velcdn.com/images/eugenieseo16/post/d72228d6-baea-4037-8c20-a33f981782a8/image.png)

저급언어는 <span style="background-color:#fff5d8;">**기계어**</span> 와 <span style="background-color:#fff5d8;">**어셈블리어**</span>  두 가지로 나뉘는데, **기계어**는 0과 1로 이루어진 명령어로 컴퓨터가 직접 실행할 수 있는 형태의 명령어이다. 그러나 기계어는 사람이 읽거나 이해하기 어렵기 때문에, 사람이 이해하기 쉬운 형태로 번역된 저급 언어와 고급 언어의 중간 단계인 **어셈블리어**가 등장했다.

### ⚡명령어의 구조

이러한 명령어는 <span style="background-color:#fff5d8;">**무엇을 대상**</span>으로, <span style="background-color:#ffc1cc;">**어떤작동을 수행하라**</span> 라는 구조 
즉, <span style="background-color:#ffc1cc;">**연산코드**</span>와 <span style="background-color:#fff5d8;">**오퍼랜드**</span>로 구성되어 있다.

- 연산 코드 : 명령어가 수행할 연산
- 오퍼랜드: 연산에 사용할 데이터 또는 데이터가 저장된 위치
  ![](https://velog.velcdn.com/images/eugenieseo16/post/0e3ad21b-f755-49b9-8e3c-98b7b5aa9cf2/image.png)

#### 🌟 연산코드

연산코드의 종류는 많지만, 기본적인 연산 코드 유형은 크게 4가지로 나뉜다.

1. <span style="background-color:#fff5d8;">**데이터 전송**</span>
   ![](https://velog.velcdn.com/images/eugenieseo16/post/fd7efc2e-6153-43fd-bf49-a7f81a1bce1a/image.png)

2. <span style="background-color:#fff5d8;">**산술/논리 연산**</span>
   ![](https://velog.velcdn.com/images/eugenieseo16/post/98d2f03d-97d9-477e-b2e8-38dcc86ceec0/image.png)

3. <span style="background-color:#fff5d8;">**제어 흐름 변경**</span>
   ![](https://velog.velcdn.com/images/eugenieseo16/post/4b47c6c5-2f06-41a2-a654-3dd1ebcc5a93/image.png)

4. <span style="background-color:#fff5d8;">**입출력 제어**</span>
   ![](https://velog.velcdn.com/images/eugenieseo16/post/60521642-234d-4ec1-adeb-808e07591b92/image.png)

#### 🌟 오퍼랜드

오퍼랜드는 연산에 사용되는 데이터 또는 데이터가 저장된 위치를 가리키기 때문에, 오퍼랜드 필드에는 숫자, 문자 데이터 또는 메모리와 레지스터의 주소 등이 들어갈 수 있다. 주로 직접적인 데이터보다는 메모리 주소나 레지스터의 이름이 담기기 때문에오퍼랜드 필드를 주소 필드라고 부르기도 한다. 

오퍼랜드는 명령어 안에 없을수도 있고, 여러개가 있을 수도 있다.
![](https://velog.velcdn.com/images/eugenieseo16/post/f8b6d15a-16da-4974-8cd6-86f5d086b6cb/image.png)

왜 오퍼랜드 필드에 메모리나 레지스터의 주소를 담을까?
➡️ 명령어의 길이 때문
오퍼랜드 안에 명령어 자체가 들어간다면, 명령어 전체의 크기가 16 비트, 연산코드 필드가 4 비트인 2-주소 명령어에서는 오퍼랜드 필드 당  6비트 밖에 담지 못하고, 표현할 수 있는 데이터는 2^6 밖에 되지 못한다.

하지만, 주소나 레지스터 이름을 담는다면 데이터의 크기는 하나의 메모리 주소에 저장할 수 있는 공간, 레지스터가 저장할 수 있는 공간 만큼 커진다.

### ⚡주소 지정 방식

데이터가 저장된 위치를 **유효주소** 라고 한다.
![](https://velog.velcdn.com/images/eugenieseo16/post/6ef76102-e373-4d4d-b32a-0a73ebe636f7/image.png)

오퍼랜드 필드에 데이터가 저장된 위치를 담을 때, 데이터 위치를 찾는 방법을 <span style="background-color:#ffc1cc;">**주소 지정 방식**</span> 이라고 한다.

CPU는 다양한 주소 지정 방식을 사용하며, 대표적인 주소 지정방식은 5가지가 있다.

#### 🌟 즉시 주소 지정 방식

![](https://velog.velcdn.com/images/eugenieseo16/post/358b24f1-f9cf-4b23-80a9-d51d59a80815/image.png)

**연산에 사용할 데이터를 오퍼랜드 필드에 직접 명시하는 방식 **
가장 간단한 형태로 적은 양의 데이터 밖에 담지 못하지만 찾는 과정이 없기 때문에 다른 주소 지정 방식 보다 빠르다.

#### 🌟 직접 주소 지정 방식

**오퍼랜드 필드에 유효주소를 직접 명시하는 방식**

즉시 주소 지정 방식보다 오퍼랜드 필드에서 표현할 수 있는 데이터의 크기는 크지만,여전히 연산 코드의 비트 수만큼 줄어들어 있다

#### 🌟 간접 주소 지정 방식

**오퍼랜드 필드에 "유효 주소의 주소"를 명시하는 방식 **

표현할 수 있는 유효 주소의 범위가 앞의 두 방법보다는 넓지만, 두번의 메모리 접근이 필요하기 때문에 더 느리다.

#### 🌟 레지스터 주소 지정 방식

직접 주소 지정 방식과 비슷하게 **연산에 사용할 데이터를 저장한 레지스터를 오퍼랜드 필드에 직접 명시하는 방법**

![](https://velog.velcdn.com/images/eugenieseo16/post/0a458680-ee86-4901-b4d6-10f75b17df73/image.png)

CPU 외부에 있는 메모리에 접근하는 것보다 내부에 있는 레지스터에 접근하는 것이 더 빠르기 때문에 빠르게 접근할 수 있다. 하지만 직접 주소 지정 방식과 비슷하게 레지스터 크기에 제한이 생길 수 있다는 문제가 발생한다.

#### 🌟 레지스터 간접 주소 지정 방식

**연산에 사용할 데이터를 메모리에 저장하고, 그 주소(유효 주소)를 저장한 레지스터를 오퍼랜드 필드에 명시하는 방식**
![](https://velog.velcdn.com/images/eugenieseo16/post/7d73ffe5-48db-4bad-9f70-3b4e68fb4d8f/image.png)

간접 주소 지정 방식과 비슷하지만 메모리 접근 횟수가 한번이다. 메모리 접근이 레지스터 접근보다 느리기 때문에 간접 주소 지정 방식보다 빠르다.

> 참고 : [혼자 공부하는 컴퓨터 구조+운영체제](https://hongong.hanbit.co.kr/%ec%bb%b4%ed%93%a8%ed%84%b0-%ea%b5%ac%ec%a1%b0-%ec%9a%b4%ec%98%81%ec%b2%b4%ec%a0%9c/)
