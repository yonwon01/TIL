# devops_실습

![스크린샷 2019-01-17 오후 4.01.25.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/VfywIvfRT0qLnKI0F0HR_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.01.25.png)

## 실습환경
  ibm pivate cloud 환경
  pipeline : git프로젝트
  
  로컬설치 준비사항
  1. cloudctl 설치
  2. kubectl 설치
  3. helm cli설치


## 1) ibm pivate cloud포탈 접속 후 로그인

## 2) namespace 작성

![스크린샷 2019-01-16 오후 3.20.11.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ZpBFE2DEQ8GPV2s8qxnj_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.20.11.png)
 > 왼쪽상단 바 클릭 후 관리->네임스페이스 접속
 
![스크린샷 2019-01-16 오후 3.20.43.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zz77IypySqZ0BM7a2Gb0_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.20.43.png)
 > 네임스페이스 입력후 작성 클릭
 
## 3) local에서 cloudctl 설치 후 로그인

![스크린샷 2019-01-16 오후 3.17.51.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/AsGYh6zRvSz1xtWG9VOn_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.17.51.png)
 > cloudctl 설치 확인 (curl -kLo <install_file> https://<cluster ip>:<port>/api/cli/cloudctl-darwin-amd64)

![스크린샷 2019-01-16 오후 3.18.52.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/1VJXY8uATZO1sPMslWMK_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.18.52.png)
> host_name입력 후 로그인 (cloudctl login -a https://"cluster_host_name":8443 --skip-ssl-validation)
[cloudctl 설치정보](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.1/manage_cluster/install_cli.html)

## 4) helm 설치

![스크린샷 2019-01-16 오후 5.09.19.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/2iOxp0zXTlia1y9qxOw7_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.09.19.png)

> helm 초기화 후 jenkins 와 sonarqube를 설치 한다.

![스크린샷 2019-01-16 오후 5.41.18.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/i3m83eMlRxypHfCpfLCO_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.41.18.png)

![스크린샷 2019-01-16 오후 5.49.28.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/gawMn51CRA6JRnhJj6Vx_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.49.28.png)

> helm init
helm repo update
helm install --name <namespace이름>-jenkins --namespace <namespace이름> stable/jenkins --tls
helm install --name <namespace이름>-sonarqube --namespace <namespace이름> stable/sonarqube --tls

###   error tip
 
   1) jenkins설치시 이미 존재하는 파일이여서 삭제 후 다시 설치 해야할때
       
![스크린샷 2019-01-16 오후 5.14.05.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/5cSGEtDNSoG7vu0MNQdv_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.14.05.png)     

![스크린샷 2019-01-16 오후 5.17.38.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/CgkqEf3FQHuGAsZ4Kj67_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.17.38.png)     

![스크린샷 2019-01-16 오후 5.18.23.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dPDRjlPXSrSHfxX3fhZt_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.18.23.png)
 > https인경우 tls를 붙여서 삭제해야한다.
  
  2) ImagePolicy가 존재하지 않을때
      
![스크린샷 2019-01-16 오후 5.04.58.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/YWyof3wkQLqjGqy44eTz_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.04.58.png)

![스크린샷 2019-01-16 오후 5.05.10.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ST4IbisWTLmizqtZZDP3_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.05.10.png)
> clusterimagepolicy에서 추가해줘야한다.

![스크린샷 2019-01-16 오후 5.33.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/7KXkPlxRAiK0jQKTUYOQ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.33.36.png)
> 생성한 이미지인 docker.io, quar.io 을 추가해준다.

## 5) jenkins 비밀번호 얻기 및 접속하기

   * 네임스페이스에 저장되어있는 secret 목록을불러온다.
   
![스크린샷 2019-01-16 오후 6.04.02.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/qhrWTor7RjiTcZgdzGrb_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.04.02.png)
 > kubectl get secret -n <namespace이름>
   kubectl get secret -n <namespace이름> <namespace이름>-jenkins -o yaml
   
  * secret에서 jenkins-admin-password정보를복사한다.
  jenkins-admin-password는 base64로 인코딩 되어있다.
  인코딩되어있는부분을decode해서 password정보를얻는다
  
![스크린샷 2019-01-16 오후 6.02.12.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/PfdZMY14TpeKGGVrZgnW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.02.12.png)
> echo "S3ZVT2IwcFlIRQ==" | base64 --decode
입력후 나온 비밀번호로 jenkins접속시 입력한다.


![스크린샷 2019-01-16 오후 6.18.23.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/smwnmFYRK70qeumuWFHA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.18.23.png)


![스크린샷 2019-01-17 오전 10.00.03.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/tDVChtYuQwOeS2qgHSro_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.00.03.png)

> jenkins 노드포트를 확인하고 접속 후 아이디:admin 과 할당받은 비밀번호를 입력한다.

## 6) Jenkins url 설정

![스크린샷 2019-01-17 오전 9.59.15.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/vpR3B8P4SUGoAnceYcNR_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.59.15.png)

> jenkins관리-> 시스템설정 클릭

![스크린샷 2019-01-17 오전 9.52.44.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/SicfY21RTi6u4w8O0MaQ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.52.44.png)


![스크린샷 2019-01-17 오전 10.01.45.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/vxzC0VQTYuvsiSnefs7S_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.01.45.png)

## 7) Jenkins Plugin 관리

![스크린샷 2019-01-17 오전 10.21.02.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/7g9JdNKSp9YpHiL0yuFQ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.21.02.png)
> Jenkins관리-> Plugin 관리 설치

![스크린샷 2019-01-17 오전 10.22.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zgwo2cFuRSaOsHRauEHT_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.22.36.png)
> 업데이트된 플러그인 목록의 목록들을 모두 클릭 후 지금다운로드 후 재시작 하기 클릭

![스크린샷 2019-01-17 오전 10.23.47.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/yzyqoQFRzWcAfbmqLlU5_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.23.47.png)
> 설치가능 목록에서 Pipeine Utility Setup 을 선택한다.(Pipeline빌드시 필요한 플러그인)

![스크린샷 2019-01-17 오전 10.23.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/3w6brSDET8qTDLxzkbHC_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.23.59.png)
> Slack Notification 이라는 플러그인을 다운받는다(빌드내용을 Slack으로 알림을 보내주는 플러그인)

![스크린샷 2019-01-17 오전 10.24.32.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/0UJbIp2xTdSLv8Il31x9_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.24.32.png)
> 플러그인 다운로드 중 인것을 확인 할 수 있다.

## 8) Docker Registry 정보 입력하기

![스크린샷 2019-01-17 오전 11.34.39.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ZNiDqgeJTe2LGEoMysxa_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.34.39.png)

> Jenkins관리 -> 시스템 설정에 들어온 후 Pipeline Model Definition 을 입력한다.
Docker Label에는 해당하는 DockerRegistry의 Label(명칭)을 입력하는 곳이고, Docker Registry는 실제 ICP Docker Registry URL을 입력한다.
Registry credentials는 Docker Registry의 ID/PW정보를 입력하는곳.

![스크린샷 2019-01-17 오전 10.40.04.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/mCPF5kmWRTSS4AirPVi6_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.40.04.png)

![스크린샷 2019-01-17 오전 10.40.21.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/uk2RfPNQBSGDV6GG7Bs0_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.40.21.png)
> Registry credentials 가 없기때문에 추가

## 9)Jenkins Pipeline를 이용한 CI/CD 실습
* git 프로젝트를 활용한다 (https://github.com/yonwon01/SumApp/)
*  deployment/jenkinsfile : jenkins의 pipeline 스크립트를 작성한 파일이다. Jenkins에서 Script로 저장할수있지만,Jenkinsfile로 관리하게 되면 형상관리서버에서 함께 관리가 되기때문에, 관리의용이성 때문에 주로 Jenkinsfile을 사용한다.(향후문제발생시 Rollback 또는 Tracing목적)

![스크린샷 2019-01-17 오전 11.19.19.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/NIqdVYXdS5yqYHHJYP6B_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.19.19.png)

> Jenkinsfile에서 Source를 받는 Stage인 'Get Source' Stage에서 경로를 현재본인의 Workspace경로로 변경.

* pipeline.properties에 저장되어 있는 환경변수 값을 변경

![스크린샷 2019-01-17 오전 11.26.48.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Oa3oEgnRXiqv096axpoT_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.26.48.png)
> namespace에는 사용하는 클러스터 이름을 입력

* 실제로 pod가 생성될 sumapp-deploy.yaml 환경변수 변경

![스크린샷 2019-01-17 오전 11.30.57.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/CtDSGNJtTfOhcWLHEEvl_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.30.57.png)
>image repository경로를 변경

* cluster rolebing
     * cluster를 binding해주기 위해서는 권한을 줘야한다.
     
![스크린샷 2019-01-17 오후 2.26.52.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/1hOS1mYvT3q6evjbcH8N_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.26.52.png)
> kubectl create clusterrolebinding <namespace>-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts:<namespace>

![스크린샷 2019-01-17 오후 2.31.46.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/L4TdSlwfRTeEvRCGg0Fg_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.31.46.png)
> test connection 클릭후 바인딩확인

* Jenkins에 Pipeline 생성하기

![스크린샷 2019-01-17 오전 11.40.57.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dHwxzBu6SazENr1y4Igd_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.40.57.png)

![스크린샷 2019-01-17 오전 11.45.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/aDm9NrOeQO6ViRJxgyPE_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.45.59.png)

![스크린샷 2019-01-17 오후 1.47.32.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zxOuU7CR6aPOoa0lzS5I_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.47.32.png)
> github에 만들어놓은 파이프라인 프로젝트 주소를 붙여넣는다
ScriptPath : Jenkinsfile 생성되있는 경로

* 만들어진 pipeline을 빌드시킨다.

![스크린샷 2019-01-17 오후 2.59.26.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/soVi6dBQWmF3CrSbuOrj_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.59.26.png)
> buildnow 클릭 후 빌드되는것을 확인한다

![스크린샷 2019-01-17 오후 2.56.31.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/koaiHOI1Q7iLDKq42KzM_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.56.31.png)


## 10) SonarQube 설정하기

* SonarQube에 로그인

![스크린샷 2019-01-17 오후 1.57.13.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/7JNJgzmGSZWEe072a9qH_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.57.13.png)


![스크린샷 2019-01-17 오후 3.06.04.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/CbZn6kmYS2e4C7jmxI6G_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.06.04.png)

![스크린샷 2019-01-17 오후 3.07.44.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/vHsy7nEvTyW8X2o4ufFU_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.07.44.png)
> 초기 설치 후 위 화면에서 받은 token값을 Generation한뒤에 해당 토큰 값을 보관한다

![스크린샷 2019-01-17 오후 3.06.47.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/66fvQqB6QOy8qmldeIrR_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.06.47.png)
> 생성된 cli 를 Jenkinsfile(파이프라인)에 붙여넣는다.

![스크린샷 2019-01-17 오후 3.13.10.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/EQiiNCPbSDaPlF3zHp9p_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.13.10.png)
> Jenkinsfile에 붙여넣었으므로 다시 빌드한다

![스크린샷 2019-01-17 오후 3.19.46.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/JVPO6gWqSs62Zc3FS2ub_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.19.46.png)

![스크린샷 2019-01-17 오후 3.21.29.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/jJoiqJb3RyOAqnYKE8y0_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.21.29.png)
> SonarQube에 정상적으로 빌드된것을 확인한다

## 11) Jenkins에 Slack 설정하기

![스크린샷 2019-01-17 오후 3.32.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/rl4XruEeSa2ImiuzV4Yh_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.32.59.png)

![스크린샷 2019-01-17 오후 3.33.47.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/POcLUs22SIi9nRfcaVNE_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.33.47.png)

![스크린샷 2019-01-17 오후 3.34.29.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/v6oGIb1MSnCg1cNLa03v_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.34.29.png)

![스크린샷 2019-01-17 오후 3.29.51.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/KF4bwCTfQMBcJu5BSEUK_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.29.51.png)

![스크린샷 2019-01-17 오후 3.36.49.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/HmjXr9IaRiyCAIgiHA5A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.36.49.png)

![스크린샷 2019-01-17 오후 3.40.48.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/mme4fhnxRt2ZjcLrZ8hI_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.40.48.png)
> 이전에 slack-notification 플러그인은 설치 했으므로 jenkins의 시스템설정에서 BaseURL과 토큰값을 입력해준다

![스크린샷 2019-01-17 오후 3.42.35.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/FjCe25W4T6uv05AMA16A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.42.35.png)

![스크린샷 2019-01-17 오후 3.42.46.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/r7JD8UUhSRKB2dW6DSxC_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%203.42.46.png)
>  git의 Jenkinsfile에 slack알림 함수를 만든다.

![스크린샷 2019-01-17 오후 4.05.49.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/54E5j3vcTeSq9K3rVhvc_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.05.49.png)
> "successful" slack alarm이 온것을 확인 할 수 있다.
