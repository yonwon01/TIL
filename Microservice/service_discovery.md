# service_discovery

#### - MSA와 같은 분산 환경은 서비스 간의 원격 호출로 구성이 된다. 원격 서비스 호출은 IP 주소와 포트를 이용하는 방식이 되는다. 클라우드 환경이 되면서 서비스가 오토 스케일링등에 의해서 동적으로 생성되거나 컨테이너 기반의 배포로 인해서, 서비스의 IP가 동적으로 변경되는 일이 잦아졌다.
![스크린샷 2019-03-11 오후 2.15.00.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/a33J4wzJRFiWJISQ0pNU_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.15.00.png)

> 그래서 서비스 클라이언트가 서비스를 호출할때 서비스의 위치 (즉 IP주소와 포트)를 알아낼 수 있는 기능이 필요한데, 이것을 바로 서비스 디스커버리 (Service discovery)라고 한다.

![스크린샷 2019-03-11 오후 2.16.42.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/gG1ZO0wgTVOqvV85Ecng_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.16.42.png)
> 위 그림을 보면 Service A의 인스턴스들이 생성이 될때, Service A에 대한 주소를 Service registry (서비스 등록 서버) 에 등록해놓는다. Service A를 호출하고자 하는 클라이언트는 Service registry에 Service A의 주소를 물어보고 등록된 주소를 받아서 그 주소로 서비스를 호출한다.

#### - service discover 종류 :
#### 1) client side discovery : service client가 service registry에서 서비스의 위치를 찾아서 호출 하는 방식
#### 2) server side discovery : 호출이 되는 서비스 앞에 일종의 proxy 서버 (로드밸런서)를 넣는 방식인데, 서비스 클라이언트는 이 로드밸런서를 호출하면 로드밸런서가 Service registry로 부터 등록된 서비스의 위치를 리턴하고, 이를 기반으로 라우팅을 하는 방식

![스크린샷 2019-03-11 오후 2.16.42.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/wlBgQleTTYaj98GHnkWS_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-11%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.16.42.png)
> 위의 server side dicsovery 방식은 클라우드에서 사용하는 로드밸런서를 생각하면 된다
