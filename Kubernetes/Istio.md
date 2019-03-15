# Istio

#### Istio 정의 : 
- Istio는 sidecar pattern을 이용한 service mesh architecture 의 구현체인 오픈 플랫폼이다.Envoy를 이용해서 서비스 매쉬를 구현하기 위해서는 Envoy로 구성된 데이타 플레인을 컨트롤할 솔루션이 필요하다. Envoy를 데이타 플레인으로 사용하고 이를 컨트롤 해주는 오픈 소스 솔루션이 Istio이다.

![스크린샷 2019-03-11 오후 5.37.30.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/cswUGrB3QO9xEP92U87W_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.37.30.png)

#### Istio 구조 :
1) 데이터 플레인 : 데이터 플레인은 사이드카 형식으로 배포되는 envoy 프록시로 구성되어 있다. 그러면 envoy는 서비스들의 IP 주소 (엔드포인트)를 어떻게 알 수 있을까? 서비스들에 대한 엔드포인트 정보는 컨트롤 플레인의 파일럿(Pilot)이라는 컴포넌트에 저장되어 있고, envoy는 이 데이타를 참고하여 엔드포인트를 알 수 있다. 
2) 컨트롤 플레인 : 컨트롤 플레인은 데이타 플레인에 배포된 envoy를 컨트롤 하는 부분으로, 파일럿 (Pilot), 믹서 (Mixer), 시타델(Citadel) 3개의 모듈로 구성이 되어 있다. 






### NetflixOSS 비교 :
- 코드상에 @EnableEurekaClient @EnableSideCar 등의 코드를 추가해야하는 NetflixOSS와 다르게 Istio는 pod 만으로 적용이 가능하다.
- Java 이외에도 사용이 가능하다 (NetflixOSS는 스프링기반)
