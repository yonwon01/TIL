# Envoy Proxy

#### - Envoy Proxy :
마이크로서비스 아키텍처구조를 그릴때 Netflix OSS툴에서 Zuul은 API Gateway 역할로 유명하다.그것과 비슷한 역할을 하는것이 Envoy proxy이다.Envoy 프록시는 Lyft사에서 개발되었으면 오픈소스로 공개되었다. 기존 프록시 L4기능 뿐 아니라 L7 기능도 지원하면서 HTTP 뿐아니라 HTTP 2.0,TCP,gRPC까지 다양한 프로토콜을 지원한다. 

#### - Envoy Proxy 주요기능 :
- HTTP, TCP, gRPC 프로토콜을 지원
- TLS client certification 지원
- HTTP L7 라우팅 지원을 통한 URL 기반 라우팅, 버퍼링, 서버간 부하 분산량 조절등
- HTTP2 지원
- Auto retry, circuit breaker, 부하량 제한등 다양한 로드밸런싱 기능 제공
- 다양한 통계 추적 기능 제공 및 Zipkin 통합을 통한 MSA 서비스간의 분산 트렌젝션 성능 측정 제공함으로써 서비스에 대한 다양한 가시성 (visibility)을 제공
- Dynamic configuration 지원을 통해서, 중앙 레파지토리에 설정 정보를 동적으로 읽어와서 서버 재시작없이 라우팅 설정 변경이 가능함
- MongoDB 및 AWS Dynamo 에 대한 L7 라우팅 기능 제공

#### Envoy 배포 아키텍처
![스크린샷 2019-03-11 오후 5.12.25.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/e59ZmmJTiyXpjCjWQSEX_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.12.25.png)

- Front envoy proxy
특정 서비스가 아니라, 전체 시스템 앞의 위치하는 프록시로, 클라이언트에서 들어오는 호출을 받아서 각각의 서비스로 라우팅을 한다. URL 기반으로 라우팅을 하는 기능 이외에도, TLS(SSL) 처리를 하는 역할들을 할 수 있다. 통상적으로 nginx나 apache httpd가 리버스프록시로 이 용도로 많이 사용되었다. 
 
![스크린샷 2019-03-11 오후 5.22.52.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/DNOhHDATLFLxEqFus2gl_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.22.52.png)


- Service to service ingress listener
특정 서비스 앞에 위치하는 배포 방식으로 서비스로 들어오는 트래픽에 대한 처리를 하는데, 트래픽에 대한 버퍼링이나 Circuit breaker 와 같은 역할을 수행한다.

- Service to service egress listener
특정 서비스 뒤에서 서비스로부터 나가는 트래픽을 통제 하는데, 서비스로 부터 호출 대상이 되는 서비스에 대한 로드 밸런싱, 호출 횟수 통제 (Rate limiting)와 같은 기능을 수행한다.

- External service egress listener
내부서비스에서 외부 서비스로 나가는 트래픽을 관리하는 역할인데, 외부 서비스에 대한 일종의 대행자(Delegator)와 같은 역할을 한다.

시스템 앞 부분이나 또는 시스템을 구성하는 서비스의 앞뒤에 배치할 수 있는 구조지만, 서비스 앞뒤로 붙는다고 실제로 배포를 할때 하나의 서비스 앞뒤로 두개의 envoy proxy를 배치하지는 않는다.
다음과 같이 하나의 서비스에 하나의 Envoy를 배치 한후, ingress/egress 두 가지 용도로 겸용해서 사용한다. 

![스크린샷 2019-03-11 오후 5.23.42.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ClzxCO1HRrKl7xieIPlr_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.23.42.png)






