### 🔥 간단한 클라우드 컴퓨팅 개념 정리

#### ⭐ **클라우드 컴퓨팅**

컴퓨터를 사용한 정보처리를 자신이 보유한 PC가 아닌 인터넷 너머에 존재하는 클라우드 사업자의 컴퓨터에서 처리하는 **서비스**

#### ⭐ 클라우드 특징

- <span style="background-color:#fff5d8;">주문형 셀프 서비스</span> : 개별 관리화면을 통한 서비스 이용
- <span style="background-color:#fff5d8;">광범위한 네트워크 접속</span> : 다양한 디바이스를 통한 서비스 접속
- <span style="background-color:#fff5d8;">리소스의 공유</span> : 여러 사용자들이 공유하는 형태
- <span style="background-color:#fff5d8;">신속한 확장성</span> : 필요한 만큼 규모 확대 및 축소 가능
- <span style="background-color:#fff5d8;">측정 가능한 서비스</span> : 이용한 만큼 요금 부과

#### ⭐ 클라우드 이용 모델

클라우드 환경을 누가. 어디에 구축하고, 운영하느냐에 따라 부르는 이름이 달라진다.
<img src="https://velog.velcdn.com/images/eugenieseo16/post/805657a7-0b3f-444f-8cfc-ddb8fc87d0a2/image.png" title="" alt="" width="399">

- <span style="background-color:#FFB6C1;">퍼블릭 클라우드</span> : 클라우드 사업자가 시스템을 구축하고, 인터넷망 등의 네트워크를 통해 불특정 다수의 기업과 개인에게 서비스를 제공하는 형태
- <span style="background-color:#FFB6C1;">프라이빗 클라우드</span> : 클라우드 서비스의 사용자 또는 사업자의 데이터 센터에 클라우드 관련기술이 활용된 자사 전용환경을 구축하여 컴퓨팅 리소스를 유연하게 이용할 수 있는 형태
- <span style="background-color:#FFB6C1;">커뮤니티 클라우드</span> : 공통의 목적을 가진 특정 기업들이 클라우드 시스템을 형성하여 데이어 센터에서 공동 운영하는 형태
- <span style="background-color:#FFB6C1;">하이브리드 클라우드</span> :퍼블릭 클라우드, 프라이빗 클라우드, 커뮤니티 클라우드 ➕ 온프레미스 시스템 

#### ⭐ 클라우드 보급 배경

: CPU의 처리 고속화, <span style="background-color:#AFEEEE;">**가상화 기술**</span>과 <span style="background-color:#AFEEEE;">**분산처리 기술**</span>의 발전, 모바일의 융성과 빠르고 저렴해진 네트워크, 거대해진 데이터 센터에 의한 규모의 경제 등 **다양한 기술의 발전**을 통해 클라우드 컴퓨팅이 보급 될 수 있었다.

### 🌞 클라우드를 실현하는 기술들

### 🔥 가상화

단일 컴퓨터의 하드웨어 요소를 논리적인 파티셔닝을 통해 다수의 가상머신(VM)으로 분할, 운영할 수 있도록 해주는 기술. 각각의 VM은 자체 운영체제(OS)를 실행하며 마치 독립적인 컴퓨터인 것처럼 작동한다.

<img src="https://velog.velcdn.com/images/eugenieseo16/post/985fd8f1-3041-4819-8d81-1f5fef47d159/image.png" title="" alt="" width="413">

- 가상화 기술 발전 이전에는, 하나의 하드웨어 시스템은 하나의 OS만 실행이 가능했다.

- Hypervisor 가상화와 Host 가상화가 있는데, 둘의 가장 큰 차이점은 가상화를 작동 사용하기 위한 Host OS의 필요 유무이다.
  
  >  **Hypervisor** : 자원을 논리적으로 분리해주는 소프트웨어. 공유 컴퓨팅 자원을 관리하고, 가상머신들을 컨트롤하는 중간관리자
  > **Host OS** : 기존에 컴퓨터에 설치되어 있었던 운영체제
  > **Guest 0S **:추가적으로 설치된 운영체제 
  
  #### ⭐ Host 가상화

- Host OS 위에 가상화를 구동 시켜 Guest OS가 구동되는 방식

- 설치와 구성이 편리하지만, 가상머신의 요청이 반드시 Host OS 통해 이뤄져야 하기 때문에 오버헤드가 발생할 수 있다.
  
  #### ⭐ **Hypervisor 가상화**

- Host OS 필요없이 직접 하드웨어에 설치하여 Guest OS를 구동 시키는 방식

- Host OS에 하드웨어의 리소스를 할당할 필요가 없어 상대적으로 오버헤드가 적다.

- <span style="background-color:#e2f0d9;">반가상화 (Para Virtualization)</span>
    guest OS가 Hypervisor를 통해 Hardware를 제어한다.
  Hypervisor가 모든것을 제어하기 때문에 높은 성능 유지가 가능하며, 여러 리소스들을 한대 처럼 운영하거나 하나의 리소스를 여러대로 나누어 탄력적이 운영이 가능하

- <span style="background-color:#e2f0d9;">전가상화 HVM (Hardware Virtual Machine)</span>
  하드웨어를 완전히 가상화 하는 방식
  Windows부터 Linux까지 다양한 OS를 사용할 수 있다.
  모든 가상머신들의 하드웨어 접근이 해당 관리 머신을 통해서 이루어지기 때문에 비교적 성능이 느릴 수있다.

<img src="https://velog.velcdn.com/images/eugenieseo16/post/fb91a149-e3ef-4b1b-8133-5ef93cc3c625/image.png" title="" alt="" width="512">

Hypervisor 가상화 방식은 여러 Guest OS를 가상화하여, 각각 독립적인 OS를 실행시켜야 한다. 또한 Hypervisor가 모든 명령을 컨트롤하고, 가상환경을 구축해야 하기 때문에 여러 OS를 구동 할수록 성능과 속도가 저하되었고, 리소스를 많이 차지하고, 부팅시간이 길어졌다.

이러한 문제를 해결하기 위해 컨테이너 가상화 방식이 등장하였다.

### 🔥 컨테이너 가상화

<img src="https://velog.velcdn.com/images/eugenieseo16/post/c15a11c3-ed58-4ab8-949d-37a68755dcee/image.png" title="" alt="" width="542">

컨테이너는 개별 소프트웨어 실행에 필요한 실행환경을 독립적으로 운용할 수 있도록 하는 기반환경이며, 다른 실행환경과의 간섭을 막고 실행의 독립성을 확보해주는 운영체계 수준의 격리 기술이다. 

컨테이너는 OS를 가상화하여 여러 개의 컨테이너를 OS 커널에서 직접 실행한다. Hypervisor와 Guest OS 없이 각각의 Application을 모듈화하여 리소스를 효율적으로 관리하기 때문에 기존의 가상화 기술보다 가볍고 빠르게 동작하며, 메모리를 훨씬 적게 차지한다.

#### ⭐ 컨테이너 가상화의 특징



**1. 배포편의성과 이동성**

개발환경과 상용환경이 다를 경우 각각의 버전에 맞는 라이브러리를 사용하게 되고, 그대로 개발 완료한 서비스를 상용환경에 올리게 되면 버전 차이에 따른 오류가 발생할 수있다.

하지만, 컨테이너는 **서비스 배포에 필요한 다양한 요소들을 하나의 이미지로 만들기 때문에 사용환경 변화에 따른 변화대응에 편리**하다. 또한 이미지만 있으면 어떠한 환경에서도 이미지를 배포할 수 있기 떄문에 이동성이 증가한다.

**2. 가벼운 가상화 기술**

Hypervisor가 아닌 **Container 엔진을 통해서 자원을 논리적으로 분리하여 사용**한다. 

Container엔진은 namespace(커널에 관련된 영역 분리)와 cgroup(자원에 대한 영역 분리)이라는 구분자를 통해 Host 서버에 대한 자원들을 구분하는데, 이를통해 Guest OS 없이 Host OS를 공유하며 Container를 배포하여 사용할 수 있다. 

하지만 Guest OS가 없기 떄문에 Host OS와 다른 COntainer의 생성이 불가능하며, VM에 비해 보안 위험성이 증가한다(VM에서는 Guest OS가 한번 더 보안을 막아주기 때문).

> **커널**: 운영체제의 핵심 부분으로써 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할을 하며 여러가지 하드웨어(CPU, 메모리) 등의 컴퓨터 자원들을 관리하는 역할을 한다. 

#### ⭐ 도커

도커는 컨테이너 기반의 가상화 오픈소스 플랫폼이다.



> 참고

1. [하야시 마사유키. 『그림으로 배우는 클라우드』. 서재원(역). 영진닷컴, 2020.](https://book.interpark.com/product/BookDisplay.do?_method=detail&sc.prdNo=337261150&gclid=CjwKCAjwxOymBhAFEiwAnodBLF0taukl4Mg5w59pVQJIpTl33ekayDVUGNUPIbfd7r2cgPfJjzw9oxoC9lkQAvD_BwE)
2. [etnews_[실용주의 클라우드컴퓨팅]](https://www.etnews.com/20191231000075)
3. [클라우드 컴퓨팅 한 방 정리 [안될과학-긴급과학 X AWS]](https://www.youtube.com/watch?v=exewHoMNjsQ)
4. [icomer_가상화](http://www.icomer.com/new/ict_solutions/ict4.html)
5. [KTcloud_가상화 기술 용어 정리](https://tech.ktcloud.com/77)
6. [KTcloud_ 컨테이너와 도커 알아보기](https://tech.ktcloud.com/77)
7. [[kt cloud Basic] 컨테이너(Container)와 도커(Docker) 알아보기](https://www.youtube.com/watch?v=hpFzsZTEPos&t=17s)
