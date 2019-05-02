# DDD

### Domain-Driven Design을 활용한 마이크로서비스 아키텍처
- 마이크로서비스는 Domain-Diven Design 아키텍처 패턴과 DevOps에서 생겨난 어플리케이션 아키텍처 스타일이다.

### Model Driven Design 한눈에 이해하기

- 최근 비즈니스환경은 매우 복잡하고 경쟁적이며 조금의 오류로 인한 잘못된 결정은 치명적인 결과를 초래할 수 있다.이러한 복잡한 도메인 모델을 해결수 있는 할 수 있는 소프트웨어 개발 접근 방법이 Domain-Driven-Design이된이 Domain-DrisiD에서 사용되는 용어는 domain,subdomains,bounded contexts,,context maps, domain models,ubiquitous langguage가 있다.

![스크린샷 2019-04-30 오후 2.45.10.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ygSOyaxRD2ZEYD0EHSSu_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-04-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.45.10.png)

- Domain= World Map
- Bounded Context= Countries (South Korea)
- Ubiquitous Language = Korean Language  (한국어hangug-eo)
- Domain Model = Map of Korea

### DDD
- Domain-Driven Design은 전략적인 가치를 기반으로한 프레임워크로써 비즈니스 도메인 개념을 소프트웨어 아티팩트에 매핑하는 것이다. 아래의 4가지 접근법을 따르면 마이크로서비스의 장점을 실행시킬 수 있다.
![스크린샷 2019-04-29 오후 2.20.22.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/jzUxSqQHSOy6FKdWFrqM_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-04-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.20.22.png)

1. Analyze Domain
     - 도메인을 현실세계에서 찾아보면 은행,신용카드,대출금,자동차 등 여러가지가 있을 수 있다. 이러한 도메인은 개발자가 구현할 시스템에 대한 요구 사항 및 허용 기준을 알려줘야 한다. 마이크로서비스 형태로 성공적으로 실행시키려면  DDD접근 방식을 통해 명백하게 도메인을 분리 시켜야 한다.
    * 팀은 보통 특정 비즈니스 영역에서 작업한다
    * 비즈니스 도메인은 팀의 핵심 부분이다
    * 도메인이 세분화 되는것은 조직의 팀 구성에 따라 달라진다
    
2. Subdomains
	   - 도메인은 subdomain으로 나눠질 수 있다. 예를들어서 자동차 도메인은 물류,연구 및 개발,무역업자,생산 및 마케팅으로 분해할 수 있다. 또한 subdomain으로 쪼개진 연구 및 개발은 디자인,CAD,테스트,보토시스템,인포테인먼트, 제품 계획 및 엔진 개발로 나눠져 subdomain이 될 수 있다.
     * 각 도메인은 subdomain으로 나눠질 수 있다
     * 애플리케이션을 도메인 및 하위 도메인에 매핑하는 것은 전통적인 엔터프라이즈 아키텍처 접근 방식이다
     * 각각의 subdomain은 또 다른 subdomain을 포함 할 수 있다
     * 전형적으로 Black-and-Witebox 모델링이다
     * 여러 도메인 사이에 커뮤니케이션이 가능하다
     ![스크린샷 2019-04-29 오후 3.28.01.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/bDZEK5gSqSbsgBSh4li5_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-04-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.28.01.png)

3. bounded contexts
    - 도메인을 정한 후에는 bounded context를 정하는것이다.Bounded Context는 모델의 경계를 결정하며 한 개의 Bounded Context는 논리적으로 한 개의 모델을 갖는다.bounded context는 ubiquitous language와 도메인 모델을 포함하고 있다.
      * 하나의 팀당 하나의 bounded context를 갖는다
      * 각각의 bounded context는 별개의 코드 레파지토리를 가지고 있다
      * Domain Model + DB schema + UI + Web Services(API)를 포함한다
      * 큰 도메인을 작은 컨텍스트로 나눈다
      * 각각의 context는 각각의 ubiquitous language와 도메인 모델을 갖는다
      * bounded contexts는 도메인관점에서 서로 쉐어링 한다

4. Context Map
      - 각각의 어플레케이션으로 된 bound contexts는 서로 소통하거나 데이터를 공유하는것이 필요하다. bound contexts는 Context Map을 통해 Domain간의 소통이 가능하다.
  
5. Entity
      - 도메인 객체의 아디덴티티를 결정할때 Entity를 통해 디자인 한다.
      * 쉐어링이 되지 않는다
      * 변할수있다 - 상태가 계속 변할 수 있다
      * 다른 entity나 value object와 연관될 수 있다
      * Entity는 history가 있고 트레이싱이 가능하다

6. Aggregate
     - 데이터 변경의 단위(트랜잭션)로 다루는 연관 객체의 묶음이다
     * 각 aggregate는 루트와 바운더리가 있다. 
     * 루트는 단 하나만 존재하며 외부에서 객체를 참조할때는 어그리게이트 루트만을 통해서 이루어져야만 한다. 
     * 삭제와 변경도 어그리게이트 루트를 통해서만 이루어져야 한다. 
     * 외부객체는 어그리게이트 루트만 접근할 수 있게 하고 내부만 감춰야만 한다

![스크린샷 2019-04-29 오후 5.20.14.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/TneDT7nZSiepw6tRBAyg_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-04-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.20.14.png)
- 위 사진에서 레고의 9부분중(2 arms, 2 hands, 2 legs, 1 pelvis, 1 body, 1 head) 한부분만 사라져도 기능을 못하는 것과 마찬가지로 Aggregate도 불변의 성격을 가지고 있어서 하나의 기능이 사라지면 제 기능을 못한다.


