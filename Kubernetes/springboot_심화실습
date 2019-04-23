# springboot_심화실습
## Kubernetes에서 실행되는 Spring PetClinic Microservice 
### * 목표 : 스프링기반으로 개발된 서비스를 마이크로서비스화 하여 쿠버네티스에 올려보는 실습을 해보고 이해한다. 
### * 실행환경 :
- kubernetes는 ibm public cloud에서 제공하는 	kubernetes service ( [kubernetes_실습구성 참고](https://torchpad.com/workspace/wikis/yonwon01/pages/Kubernetes_%EC%8B%A4%EC%8A%B5%EA%B5%AC%EC%84%B1 "kubernetes_실습구성 참고") )
- local에 kubectl 설치하기 (필자는 mac사용) : https://kubernetes.io/docs/tasks/tools/install-kubectl/ 
- local 에 MySQL Database 설치하기  
- local 에 Maven 설치하기 
- 파일 편집용도로 atom사용 (atom 설치 : https://atom.io/)   

## 1. git에서 spring기반으로 제작된 PetClinic 을 로컬 터미널에 clone한다.
![스크린샷 2019-01-24 오후 4.02.20.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/drwasFKRa6AgNXdhGhWg_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.02.20.png)
> git clone https://github.com/yonwon01/spring-petclinic-kubernetes.git

## 2. spring-petclinic-kubernetes가 복제된 폴더로 들어와서 메이븐 빌드를 한다.
  * 스프링은 maven build를 지원한다.
  
![스크린샷 2019-01-24 오후 4.09.16.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/WuuhZS5dTQeVbOz8gkLv_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.09.16.png)
> mvn install

  * build를 성공하면 각 서비스폴더에 target폴더가 생성되고 target안에 jar파일이 생성된것을 확인할 수있다.  
  
![스크린샷 2019-01-24 오후 4.15.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Hssdq1T0S9W5w1Es3jsg_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.15.36.png)
> ls
cd spring-petclinic-api-gateway/
cd target
ls

## 3. docker 빌드하기
  * 이미지파일은 어느 docker registry 들었냐에 따라 다르게 동작한다.
  * 따로 tag처리를 하지 않으면 docker hub을 바라본다.
  
![스크린샷 2019-01-24 오후 4.22.34.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/vlsHjC2jQEaFMXkLF9Zu_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.22.34.png)
> docker hub의 registry경로에 맞게 편집해준다.
$ docker build . -t dockerHub아이디/생성할 레지스트리이름

 * Dockerfile확인하기
 
![catDockerfile_4.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/YjG0xZiQxe8dFRT3YN6t_catDockerfile_4.png)
> cat Dockerfile

 * build.sh 확인하기
 
![catSH_5.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/fKbVFQ5JTLqESkxTTgSY_catSH_5.png)
> cat build.sh

 * docker 빌드하기
 
![dockerHub에빌드하기_6.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/900d5632SZSfh2iCOH0B_dockerHub%E1%84%8B%E1%85%A6%E1%84%87%E1%85%B5%E1%86%AF%E1%84%83%E1%85%B3%E1%84%92%E1%85%A1%E1%84%80%E1%85%B5_6.png)
> ./build.sh 
spring-petclinic-api-gateway 외 3개 폴더 도 똑같이 경로 변경 후 build 해준다.

 * 빌드 sh 와 마찬가지로 push sh도 registry 모두 변경해준다.
 
![스크린샷 2019-01-24 오후 4.38.13.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/3Z2eNydT1m6OnxUzoE34_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.38.13.png)
 
 *  빌드된 것을 docker hub으로 push 하기 
 
![push.sh_6.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/jWRB7bdSfOOgJBNVy0tg_push.sh_6.png)
> ./push.sh
spring-petclinic-api-gateway 외 3개 폴더도 똑같이 경로 변경 후 push 해준다.

 * docker 에 생성된 image확인
 
![스크린샷 2019-01-24 오후 4.47.04.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/3DvmOaooSh20MsCoUg2J_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.47.04.png)
> docker images

![dockerhub에 다올라옴.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Vda9c28QSEG1jRimTVRp_dockerhub%E1%84%8B%E1%85%A6%20%E1%84%83%E1%85%A1%E1%84%8B%E1%85%A9%E1%86%AF%E1%84%85%E1%85%A1%E1%84%8B%E1%85%A9%E1%86%B7.png)
> docker hub 에 모두 올라온것을 확인할수있다.

## 4. kubernetes에 mysql서버 올리기 

* mysql폴더로 들어간다.

![스크린샷 2019-01-24 오후 5.04.46.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/F5YlXeIDT26WP2gRvfjg_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.04.46.png)


* Persistent Volume을 생성한다.
   * MySQL 서버가 내용을 저장할 공간을 할당한다. 클러스터 제공 환경에 따라 다양한 영구 저장소를 제공하지만, 가장 간단한 것은 Node 서버의 파일 시스템 공간을 이용하는 것이다. 이를 위해 다음과 같은 명령으로 `local-volume`이란 이름의 5Gi의 저장 공간을 생성한다.
   
![mysql-volume생성_7.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/m7V6XX4MRA29KItQn6ue_mysql-volume%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_7.png)
> kubectl create -f local-volumes.yaml 

![localvolume_7.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/gwBxOg7rQXTPT9Xjckag_localvolume_7.png)

![get_pv_7.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/2G0FwltmQAymBnzK0KU3_get_pv_7.png)
> kubectl get pv 


* Persistent Volume Claim을 생성한다.
   * Volume 을 대상으로 어느만큼의 용량을 사용할 건지 정의 한다.
   
![pvc_8.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zT3z0OXgR7WF4tHP4s54_pvc_8.png)
> kubectl create -f mysql-pv-claim.yaml (1기가만 달라했지만 free테스트여서 5기가밖에주는기능 밖에없음)
kubectl get pvc

* secret등록하기
    * mysql 사용자와 비밀번호는 ConfigMap이 아닌 Secret으로 입력합니다. 파일에서 생성하기 위해 다음과 같이 입력한다.
  
![스크린샷 2019-01-24 오후 5.35.48.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/PgEspcceRyy61jQ5l3RC_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.35.48.png)
> echo -n "root" > ./username
echo -n "petclinic" > ./password
kubectl create secret generic mysql-credential --from-file=./username --from-file=./password
   
![스크린샷 2019-01-24 오후 5.36.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ay6SFmxhRFOBu9OAy7PP_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.36.36.png)
> kubectl get secret mysql-credential -o=yaml

   
* mysql 디플로이멘트 파일과 mysql 서비스 파일을 생성한다. 서비스의 selector의 라벨이름을 가진 deployment를 찾아서 pod와 연결된다. 즉,외부에서 접속할때 service의 노드port를 통해 접속하고 서비스의 selector를 통해 pod와의 접속을 할 수있다.

![mysql_deployment_생성_9.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ypos2MbTQw24KFqevXP7_mysql_deployment_%E1%84%89%E1%85%A2%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_9.png)

> kubectl create -f mysql.yaml
kubectl create -f mysql-service.yaml

## 5. 생성된 mysql에 데이터 입력하기
 * 공용ip와 mysql 노드 포트를 통해 쿠버네티스에 생성한 mysql디비에 접속하여 데이터를 입력한다.
 
![스크린샷 2019-01-24 오후 5.52.39.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dTP1TLGS1KY9MZqjEDi4_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.52.39.png)
 > echo $EXTERNAL_IP
 mysql -h $EXTERNAL_IP -u [userid] -p -P 32001 petclinic < spring-petclinic-customers-service/src/main/resources/db/mysql/schema.sql
 참고 : https://gist.github.com/jgkong/0efebe5da25c2e369ce6a7334b542052
 
 * 데이터가 들어왔는지 확인한다.
 
![db접속.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/gBV1sI99RxS4m3nihaNA_db%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8.png)
 
![db접속_2.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/eUdwpuhaSuqUeYM78oep_db%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8_2.png)

![db접속_3.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/apqLAD0RBqNbjtaIf7EG_db%E1%84%8C%E1%85%A5%E1%86%B8%E1%84%89%E1%85%A9%E1%86%A8_3.png)
> 데이터가 제대로 생생된것을 확인할 수있다.

* MySQL 호스트 서버 및 포트 정보 입력하기
> kubectl create -f k8s/configmap.yaml

## 6. 각각의 마이크로서비스들을 배포하고 서비스 하기
  *  docker registry secret생성하기 전에 deploy-api.yaml에 imagePullSecrets을 넣어준다.
  imagePullSecrets은 public registry(docker hub)이 아닌경우 
  secret정보를 넣어주어 접속 할 수 있도록 하는 역할
  
![스크린샷 2019-01-24 오후 6.15.07.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/uMi5m5RautBRuLxpch5A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.15.07.png)



  * docker registry secret 생성하기
  
![스크린샷 2019-01-28 오전 11.02.50.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/G2MY1hr5TXivczFbBhy1_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-28%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.02.50.png)
> Kubectl create secret docker-registry regcred --docker-server=hub.docker.com --docker-username=yonwon01 --docker-password=seowon1! --docker-email=seowchoi@kr.ibm.com



 * 각 4가지의 마이크로서비스들의 pod와service 파일을 생성하기
 
        1) API 마이크로 서비스 생성하기
        다음 명령을 실행하여 API 마이크로 서비스 Deployment와 Service를 생성.

> kubectl create -f ./k8s/deploy-api.yaml
kubectl create -f ./k8s/svc-api.yaml

         2) Customers 마이크로 서비스 생성하기
         다음 명령을 실행하여 Customers 마이크로 서비스 Deployment와 Service를 생성.

>kubectl create -f ./k8s/deploy-customers.yaml
kubectl create -f ./k8s/svc-customers.yaml

         3) Vets 마이크로 서비스 생성하기
         다음 명령을 실행하여 Vets 마이크로 서비스 Deployment와 Service를 생성.

> kubectl create -f ./k8s/deploy-vets.yaml
kubectl create -f ./k8s/svc-vets.yaml


         4) Visits 마이크로 서비스 생성하기
         다음 명령을 실행하여 Visits 마이크로 서비스 Deployment와 Service를 생성.

> kubectl create -f ./k8s/deploy-visits.yaml
kubectl create -f ./k8s/svc-visits.yaml

  * 정상적으로 pod가 생성된 것을 확인 
  
![스크린샷 2019-01-24 오전 11.21.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/BsB6ULQiQBOdLvTjGeTQ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-24%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.21.59.png)

## 7. nginx 배치,서비스 생성 후 노드포트를 통해 웹사이트 접속
   * ibm cloud에서 제공하는 free 클러스터는 ingress를 지원해주지 않기때문에 nginx의 노드포트를 통해 접속한다.즉 Spring PetClinic은 배포된 Service를 nginx를 이용하여 접근하는 방식을 이용한다.
   
   * nginx 의 deployment,service,configmap 파일을 실행한다.
   
![nginx_created.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/iwixxQi8SnGf9gQzggDn_nginx_created.png)
   > kubectl create -f ./k8s/nginx/nginx-configmap.yaml
kubectl create -f ./k8s/nginx/nginx-service.yaml
kubectl create -f ./k8s/nginx/nginx.yaml

  * public ip와 nignx 의 노드 포트 32010 통해 웹 브라우저를 실행
  
![pet-clinic화면.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/E4T8IdsEQcGVfUh9j8KE_pet-clinic%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB.png)
 > http://\<EXTERNAL-IP\>:32010/
