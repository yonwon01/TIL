# LoadBalancing
### LoadBalancing 정의 : 부하분산 또는 로드 밸런싱(load balancing)은 컴퓨터 네트워크 기술의 일종으로 둘 혹은 셋이상의 중앙처리장치 혹은 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것을 의미한다.

### - LoadBalancer 종류 : 
#### 1. 운영체제 로드밸랜서 : 물리적 프로세서 간에 작업을 스케줄링
#### 2. 네트워크 로드밸랜서 : 사용 가능한 백엔드에서 네트워크 작업을 스케줄링

### - 네트워크 로드밸랜서
* L2 : OSI 레이어 2에 속하는 MAC 어드레스를 참조하여 스위칭하는 장비
* L3 : OSI 레이어 3에 속하는 IP주소를 참조하여 스위칭하는 장비
* L4 : OSI 레이어 3~4에 속하는 IP 주소 및 TCP/UDP 포트 정보를 참조하여 스위칭하는 장비
* L7 : OSI 레이어 3~7에 속하는 IP 주소, TCP/UDP 포트 정보 및 패킷 내용까지 참조하여 스위칭함


![스크린샷 2019-03-06 오전 11.30.55.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/tr7oQlZMTKqDs1bI2NQA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.30.55.png)

#### - L4 :
L4(Transport Layer)특징
- Transport Layer(IP+Port) Load Balancing
- 예시: IP+Port > 213.12.32.123:80, 213.12.32.123:1024
- TCP, UDP Protocol
- 장점 : Port기반 스위칭 지원, VIP를 이용하여 여러대를 한대로 묶어 부하분산
- 주로 Round Robin 방식 사용

L4 동작 방식 :

![스크린샷 2019-03-06 오후 2.06.23.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/f2MnNYi6S7u96vfekncw_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.06.23.png)
> user가 브라우저에서 입력한 DNS주소를 l4로 요청 -> 획득한 DNS주소를 기반으로 http요청 -> l4는 내부 알고리즘(라운드로빈 등)을 통하여 서버 선별 및 요청

#### - L7 :
L7 특징
- L7(Application Layer)
- Application Layer(사용자 Request) Load Balancing
- 예시 : IP+Port+패킷 내용 >
 213.12.32.123:80, 213.12.32.123:1024 + GET/ img/aaa.jpg
- HTTP, FTP, SMTP Protocol
- 스위칭 방식 : URL 스위칭, Cookie 스위칭, Content 스위칭

L7 동작 방식 :

![스크린샷 2019-03-06 오후 2.21.15.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/e6ggy54wTTrqJ97lEm9j_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.21.15.png)
> user가 브라우저에서 입력한 DNS주소를 l7로 요청 -> URL주소에서 특정 String을 검사하고(URL스위칭), 검색된 문자열을 기준으로 사용할 서버를 분산시킴

#### - L4 vs L7 :
공통점 : 스위치로 들어온 패킷을 적절한 목적지로 전송해줌 
차이점 :
  - 기능과 역할은 동일하나 패킷을 분석하는 인텔리전스가 다름
  - L7은 L7 Layer만 다루지 않고 L2, L3기능을 포함하고 부분적인 L4 스위치 기능을 지원
  - L7은 L4의 서비스 단위 로드밸런싱을 극복하기 위한 포트 + 페이로드 패턴을 이용하여 패킷스위칭
  - L4는 TCP/UDP 패킷 정보를 분석하고 해당 패킷이 사용하는 서비스 종류별(HTTP, FTP 등)로 처리.(L4 Mega Proxy 문제 발생)

#### - sticky session : 
L4 스위치를 통해 분배된 서비스 세션은 하나의 연결 요청에 1~n 중에 한 대의 서버에 분배된다.
여러 번 시도해도 그 때마다 1~n 중에 한 대에 분배되므로, 같은 서버에 접속될 확률은 1/n이 된다.
그러나 처음에 접속했던 서버와 같은 서버에 계속 연결시킬 수 있다.
바로  sticky 옵션이다.

i) (일반적인 상태)
- 사용자A -> L4 -> 1번서버
- 사용자A -> L4 -> 3번서버

ii) (sticky 상태)
- 사용자A -> L4 -> 1번서버
- 사용자A -> L4 -> 1번서버

기존 사용자의 세션 상태를 timeout 시간 내에는 계속 유지시켜주는 것이 sticky session이다.(timeout 시간은 60분 이내로 조절 가능하다.) 
- 문제점 : 개별 사용자가 사용할 경우에는 세션 timeout이 있으므로 어느 정도 로드밸런싱을 충족시킨다.
하지만 프록시서버를 사용하는 경우 문제가 된다.
예를 들어 회사에서 외부로 나가는 경우 각 PC의 IP가 아니라 프록시서버의 IP를 달고 나간다.
여러 사람이 timeout 시간 내에 접속하는 경우, 계속해서 한 서버에만 로드가 집중된다.
(외부에서 보기에는 동일한 사람으로 보이므로)

![스크린샷 2019-03-06 오후 3.02.47.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/8u5HZ6SRK1k1YXDu2S1w_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.02.47.png)

- 대안 :
SSL이나 기타 다른 보안모듈을 이용해서 인증된 특정 사용자에 대해서 Cookie/DB에 기록 후 해당 사용자에 대해서만 세션을 유지하도록 한다. (단점 : performance 저하 및 기타 cost)-> 그래서 L7을 사용한다.

#### - L7 스위칭 방식 

- URL 스위칭
URL 주소에서 특정 String을 검사하고, 검색된 문자열을 기준으로 부하를 분산시키는 방식이다.
http://www.test.com/test.html 이라는 주소로 사용자들이 웹페이지를 요청한다.
해당 페이지는 이미지가 빈번히 변경되고, 이미지 크기도 크다. (전체적으로 로딩이 느리다)
이런 경우, client의 http request 내용에 html이 들어가면, 메인 웹서버로 전송하고 해당 request에 jpg 등의 이미지를 요청하는 경우 이미지 웹서버로 분산할 수 있다.

- content 스위칭
Http header의 cookie 값에 따른 특정 String을 기준으로 부하를 분산하는 방식이다.
Cookie 값 필드를 보고 설정된 분류 기준에 따라 어느 서버로 보낼지 결정한다.


----
* 참고
https://www.freeism.co.kr/wp/archives/698
