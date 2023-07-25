### 🔥 클라우드 컴퓨팅

#### ⭐ 클라우드 컴퓨팅의 개념

스토리지, 서버, 애플리케이션 등을 인터넷을 통해 제공하는 구축 모델
물리적 장소가 아닌 로컬 시스템과 프라이빗 데이터 센터를 주로 대체하는 IT 리소스 관리 방식
![](https://velog.velcdn.com/images/eugenieseo16/post/d4d44b35-5ebc-4a05-9f1e-bf9d1196bfcb/image.png)

**<span style="background-color:#fff9d7;">
  IT 시스템을 직접 구매하는 대신 임대하여 사용할 수 있는 서비스.
  인터넷을 통해 IT 자원과 인공지능 서비스, 빅데이터, IoT와 같은 서비스드들을 원할 때 언제든지 사용할 수 있고, 보통 사용한 만큼만 요금을 내는 온디맨스 서비스 형태로 제공된다.
</span>**

> **온프레미스 방식** **(On-premise)**
> 기업이 **자체적으로 **보유한 공간에 물리적으로 하드웨어 장비를 가지고 **직접 인프라를 구축**하여 운영하는 방식. 필요한 시스템을 구축하기 위해서 하드웨어와 소프트웨어를 구입하여, 시스템 구성 상황에 맞게 환경을 구성하는 방식이다. 인프라를 구축하기 위한 기간과 시스템을 구축하기 위한 비용이 들어가며,관리 및 운용을 위한 유지보수 비용 또한 필요로 한다. 클라우드 이전에 가장 일반적으로 사용되던 방식이다.
> **( ↔ 오프프레미스)**

#### ⭐ 클라우드 컴퓨팅의 시작

- 1965년, 과학자 존 매카시가 하나의 컴퓨터를 여러명의 유저가 공유하며 동시에 작업을 펼치는 방식으로 값비싼 메인프레임을 대체할 수 있다는 주장을 펼치면서 이 개념이 유래되었다.
- 이후 90년대까지, 클라우드 컴퓨팅의 주요 기술인 가상화(한 개의 운영체재로 여러 개의 독립적인 환경을 구축하는 것) 소프트웨어가 개발되어 발전되었다.
- 2006년, Amazon AWS가 출시되었다. 첫 번째로 시작된 서비스는 사람들이 컴퓨터에 액세스하여 클라우드에서 자신의 응용프로그램을 실행할 수 있게 해주는 Elastic Compute 클라우드 (EC2)였다. 
  ![](https://velog.velcdn.com/images/eugenieseo16/post/b493e94d-37f8-4d55-af38-d0682961b344/image.jpg)

#### ⭐ 클라우드 컴퓨팅의 장점

**<span style="background-color:#fff5d8;">
경제성, 민첩성, 비즈니스 집중 가능
</span>**
![](https://velog.velcdn.com/images/eugenieseo16/post/39e9f339-9316-478c-95aa-4657f2bfaafc/image.jpg)

**1. 운영 비용 절감**
컴퓨팅 리소스를 사용할 때만 요금을 내고, 사용한 만큼만 지불하며 규모의 경제를 통한 지속적인 가격인하가 가능하다.
**2. 탄력적 운영**
필요한 인프라 용량을 추정할 필요 없이 수요에 맞춰 유연한 확장이 가능하다. 배포 전에 용량을 결정하면, 유휴 상태의 장비가 발생하거나 한정된 용량으로 작업을 해야한다. 클라우드 컴퓨팅을 사용하면 필요한 만큼 확장하거나 축소할 수 있다.
**3. 속도, 민첩성, 안정성**
새로운 IT 리소스, 최신기술을 별도의 개발 없이 적용할 수 있다. 이에 따라 실험 및 개발에 드는 비용이 절감되고 시간이 단축되므로 조직의 민첩성이 크게 향상된다.
**4. 데이터 센터 운영 및 유지 관리에 비용 감소**
인프라 관리업무를 최소화 함으로써 서비스 개발에만 집중할 수 있다. 
**5. 글로벌 확장성**
세계 곳곳에 손쉽게 배포가 가능하기 때문에 몇 분안에 글로벌 서비스 구현이 가능하다.

### 🔥 클라우드 컴퓨팅의 유형

![](https://velog.velcdn.com/images/eugenieseo16/post/e3bcf0da-4f35-408c-b3d2-3d643c01f068/image.jpg)
⭐ 전통적인 IT > **온프레미스 롼경** : 기업에서 직접 모든 인프라를 관리

#### ⭐ Iaas (Infrastructure as a Service)

**<span style="background-color:#fff5d8;">
클라우드 업체에서 물리환경을 관리하는 방식의 클라우드 유형.</span><br>**
![](https://velog.velcdn.com/images/eugenieseo16/post/e17af4b1-727a-416b-8df5-1df22a1a5346/image.png)

가장 유연한 클라우드 서비스로 클라우드 업체에서 **하드웨어 및 소프트웨어를 소유 및 운영**하고, **네트워크, 서버, 가상화 및 스토리지**의 관리와 액세스를 담당한다. 사용자는 운영 체제 및 데이터, 애플리케이션, 미들웨어 및 런타임을 담당한다. 

운영체제를 직접 구축해야하는 번거로움이 있다 > PaaS

- **사용하는 경우** :
  - 온프레미스 보유 > 클라우드로 전환을 고려하는 경우
  - 특정 버전의 소프트웨어를 써야하는 경우
    **- Amazon Web Service(AWS), Microsoft Azure, DigitalOcean, Google Compute Engine(GCE)**

#### ⭐ Paas (Platform as a Service)

**<span style="background-color:#e6ffd8;">
개발자가 서버, 운영체제부터 네트워킹, 스토리지, 미들웨어, 도구 등 애플리케이션을 빌드, 실행, 관리하는 데 필요한 모든 것을 포함하는 클라우드 환경.</span><br>**

PaaS는 클라우드 업체에서** OS, 미들웨어, 런타임과 같은 소프트웨어 작성을 위한 플랫폼을 가상화하여 제공하고 관리**한다.** 사용자는 애플리케이션 개발과 데이터 관리를 담당**한다. 소프트웨어 업데이트 또는 하드웨어 유지관리와 같은 번거로움을 없앨 수 있으며, 기본 소프트웨어 구성 요소를 활용하여 자체 애플리케이션을 개발할 수 있어 자체적으로 작성해야 하는 코드의 양을 줄일 수 있다.

- **사용하는 경우** :
  
  - 온프레미스 미보유 고객
  
  - 클라우드가 익숙한 고객
  
  - 운영 인력이 부족한 경우
    **- AWS Elastic Beanstalk, Windows Azure, Heroku, Google App Engine**
    
    #### ⭐ Saas (Software-as-a-Service)
    
    **<span style="background-color:#caf0fa;">
    클라우드 서비스로 제공되는 소프트웨어</span><br>**
    ![](https://velog.velcdn.com/images/eugenieseo16/post/966f4ae9-9ef5-4479-b2d9-a3932dfacb29/image.png)

가장 포괄적인 형식의 클라우드 컴퓨팅 서비스로, 클라우드 제공업체가 클라우드 애플리케이션 소프트웨어를 개발 및 유지 관리하고, 자동 소프트웨어 업데이트를 제공한다.

- **사용하는 경우** :
  
  - 비즈니스에 집중이 중요한 고객
  
  - IT 인력, 개발자가 부족한 상태
  
  - 협업이 필요한 단기 프로젝트
  
  - On-premise 솔루션은 모바일 액세스를 지원하지 않기 때문에 모바일 액세스가 필요한 경우
    **- Google Apps, Dropbox, Salesforce**
    
    #### ⭐ 서비스 예시
    
    ![](https://velog.velcdn.com/images/eugenieseo16/post/cd7a0974-1d40-41bd-9144-2e9635da23a5/image.png)

- [Gatner(가트너 매직 쿼트런트 리포트)](https://www.gartner.com/en) : 클라우드 업체의 서비스 및 시장점유율 확인 가능

- [G2](https://www.g2.com/) : 기업 상황에 따른 Saas 솔루션 정보 확인 가능
  
  

> 참고:
> [Hewlett Packard Enterprise Development](https://www.hpe.com/kr/ko/what-is/cloud-computing.html) 
> [IGLOO Corp](https://www.igloo.co.kr/security-information/%ED%81%B4%EB%9D%BC%EC%9A%B0%EB%93%9C-%EC%BB%B4%ED%93%A8%ED%8C%85cloud-computing%EC%9D%98-%EA%B3%BC%EA%B1%B0-%ED%98%84%EC%9E%AC-%EB%AF%B8%EB%9E%98/)
> [whatap](https://www.whatap.io/ko/blog/9/)
> [docs.aws.amazon](https://docs.aws.amazon.com/ko_kr/whitepapers/latest/aws-overview/types-of-cloud-computing.html)
> [oracle](https://www.oracle.com/kr/cloud/what-is-cloud-computing/#cloud-computing-benefits)
> [클라우드의 서비스 모델 by 투마일스 - 2miles ](https://www.youtube.com/watch?v=kcyRa2SPxQw)
