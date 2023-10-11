클라우드에 대한 공부를 진행하면서 컴퓨터 구조와 운영 체제에 대한 지식의 중요성을 깨달았다. 특히 도커 실습 중 여러 오류를 경험하며 컴퓨터에 대한 깊은 이해의 필요성을 느꼈다. 그래서 이번 포스팅에서는 컴퓨터의 기본 구조와 용어에 대해 간략히 정리하려고 한다.

### 🔥 컴퓨터의 4가지 핵심 부품

컴퓨터는 크게 <span style="background-color:#fff5d8;">**중앙처리장치**</span>(CPU; Central Processing Unit), <span style="background-color:#fff5d8;">**주기억장치**</span>main memory(메모리), <span style="background-color:#fff5d8;">**보조기억장치**</span>(secondary storage), <span style="background-color:#fff5d8;">**입출력장치**</span>(input/output(I/O) device)로 구성되어 있다.
<img src="https://velog.velcdn.com/images/eugenieseo16/post/fd7c0dca-91a1-4bf0-85ca-f4c40199a1f7/image.png" title="" alt="" width="388">

#### 1. CPU

<span style="background-color:#e2f0d9;">**"컴퓨터의 두뇌"**</span>
<img src="https://velog.velcdn.com/images/eugenieseo16/post/ae3264d4-7377-4cf0-98f3-c68d2ca2d888/image.png" title="" alt="" width="279">

컴퓨터 시스템을 통제하고, 프로그램의 연산을 실행 · 처리하는 가장 핵심적인 컴퓨터의 제어 장치이며 기억, 해석, 연산, 제어라는 4대 주요 기능을 관할하는 장치이다. 메모리에 저장된 명령어를 읽어 들이고, 읽어 들인 명령어를 해석하고, 실행한다. 

**CPU** 는 ALU(산술논리연산장치), 레지스터(register), 제어장치(CU; Control Unit) 등으로 구성되어 있다.
✅**ALU** : 말 그대로 산술과 연산을 위한 부품이다. 컴퓨터 내부에서 수행되는 대부분의 계산은 ALU가 수행한다.
✅**register** : 프로그램을 실행하는 데 필요한 값들을 임시로 저장하는 작은 임시 저장장치이다.
✅**제어장치** : 컴퓨터의 모든 동작을 제어하는 CPU의 핵심 부분.주기억 장치, 입출력 장치, ALU에 프로세서가 전송한 명령어를 수행하도록 하는 역할을 한다.

🕹️CPU가 명령어를 실행하는 과정
![](https://velog.velcdn.com/images/eugenieseo16/post/7fbd00af-25a9-4e50-a696-c94922f4beb5/image.png)

1. 제어장치가 메모리 0번 인덱스에 저장된 명령어를 읽기위해 "메모리 읽기"라는 제어신호를 메모리에 보낸다.
2. 메모리는 CPU에 명령어를 넘기고, 메모리는 레지스터에 저장된다.
3. ALU는 읽어들인 데이터로 연산을 수행하고, 레지스터에 결과값을 저장한다. 
4. 제어장치가 명령어를 해석한 뒤, 메모리에 결과값을 저장해야 된다고 판단하면 "메모리 쓰기" 제어신호와 함께 값을 보낸다.

#### 2. 주기억장치

<span style="background-color:#e2f0d9;">**"메모리"**</span>
<img src="https://velog.velcdn.com/images/eugenieseo16/post/34dca6de-fd0d-4ce4-83d8-7a42a32dff68/image.png" title="" alt="" width="347">

CPU에 대해 설명할 때, 나온 "메모리"가 바로 주기억장치이다. 주기억장치에는 크게 **RAM**(Random Access Memory)과 **ROM**(Read Only Memory)이 있는데, 메모리라는 용어는 보통 RAM을 지칭한다. 메모리는 현재 실행되는 프로그램의 명령어와 데이터를 저장하는 부품이다. 메모리에 저장된 값의 위치는 **주소**로 알 수 있다.

#### 3. 보조기억장치

<img src="https://velog.velcdn.com/images/eugenieseo16/post/1b8c4fac-6c50-4432-ab1c-cb9b3eeb450f/image.png" title="" alt="" width="349">
주기억장치의 용량 부족을 보완하기 위하여 쓰는 외부 기억 장치이다. 대표적인 보조기억창치는 **HDD**(Hard Disk Driver)와** SSD**(Solid State Driver)가 있다. 주기억장치는 가격이 비싸 저장 용량이 적고, 전원이 꺼지면 저장된 내용을 잃는다. 이 때문에 메모리보다 크기가 크고, 전원이 꺼져도 저장된 내용을 잃지 않는 메모리를 보조할 저장 장치가 필요하다.

#### 4. 입출력장치

<img src="https://velog.velcdn.com/images/eugenieseo16/post/a8c4b53b-0645-43d8-99b3-8e34bd89bf43/image.png" title="" alt="" width="363">

마이크, 스피커, 프린터, 마우스, 키보드처럼 컴퓨터 외부에 연결되어 컴퓨터와 사용자 사이의 정보를 교환할 수 있는 장치들을 말한다.

---

이러한 핵심부품들은 모두 메인보드라는 판에 연결된다. 메인보드 내부에 있는 "버스"라는 통로로 메인모드에 연결된 부품들끼리 서로 정보를 주고 받을 수 있다. 버스 가운데 네 가지 핵심 부품을 연결하는 가장 중요한 버스는 <span style="background-color:#fff5d8;">**시스템 버스**</span>이다. **시스템 버스**는 주소를 주고받는 통로인 **주소 버스(address bus)**, 명령어와 데이터를 주고받는 통로인 **데이터 버스(data bus)**, 제어 신호를 주고받는 통로인 **제어 버스(control bus)**로 구성되어 있다. 
![](https://velog.velcdn.com/images/eugenieseo16/post/9b0ccff9-6e4c-49fe-bc13-a28bcd2188fb/image.png)

🕹️다시 보는 CPU가 명령어를 실행하는 과정

제어장치가 메모리 0번 인덱스에 저장된 명령어를 읽기위해 "메모리 읽기"라는 제어신호를 메모리에 보낸다.
➡️ 제어 버스로 ‘메모리 읽기’ 제어 신호를 보내고, 주소 버스로 읽고자 하는 주소를 보낸다. 메모리는 데이터 버스로 CPU가 요청한 주소에 있는 내용을 보낸다.

> 참고 : [혼공 -컴퓨터의 4가지 핵심 부품](https://hongong.hanbit.co.kr/%EC%BB%B4%ED%93%A8%ED%84%B0%EC%9D%98-4%EA%B0%80%EC%A7%80-%ED%95%B5%EC%8B%AC-%EB%B6%80%ED%92%88cpu-%EB%A9%94%EB%AA%A8%EB%A6%AC-%EB%B3%B4%EC%A1%B0%EA%B8%B0%EC%96%B5%EC%9E%A5/)
