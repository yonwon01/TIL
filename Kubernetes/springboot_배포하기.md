# springboot_pod배포
 - **목표** : SpringBoot 를 이용한 application을 pod에 배포하고 서비스 실행하기
 - **실행환경**  : 
       * kubernetes-cluster환경(ibm public cloud) 
       * eclipse 
       * docker,docker hub  
       * kubectl설치
  
  
## 1. 이미지를 생성할 웹페이지 생성한다.
   * spring boot rest api application 을 다운받는다. ( [Spring app API 홈페이지](https://spring.io/guides/gs/rest-service/ "Spring app API 홈페이지") )
   * eclipse에 import해서 실행시킨다.

![스크린샷 2019-01-23 오전 11.30.00.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/AP1QJZ5sRaGGsw5MKNfX_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.30.00.png)

## 2. eclipse에 다운받은 프로젝트의 위치로 들어간다.

![스크린샷 2019-01-23 오전 9.59.50.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/k7pIHHKeQ1yBozQ8NIhn_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.59.50.png)

## 3. dockerfile를 작성한다.

![vi-dockerfile.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Uwkh9huQ3itD0XBNeSz8_vi-dockerfile.png)
![dockerfile.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/YMUWPUQXRse6EGdjGE74_dockerfile.png)

## 4. Dockerfile 이미지 생성한다.

![docker image compeleyted.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/TRCDLdLeRcazXIBbfbuB_docker%20image%20compeleyted.png)
> $ docker build -t app .

![스크린샷 2019-01-23 오전 10.03.53.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/3rJ3iZX5SjGQ53DM5ARF_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.03.53.png)

## 5. 도커이미지 태깅 후 docker hub으로 push한다.

![imsge push.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/kDJ4kkuSTEWfHU1zZsvt_imsge%20push.png)


## 6. docker hub에서 push된것을 확인한다.

![docker_hub.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dVlT0OfySbe0zwjx9c8N_docker_hub.png)

## 7. docker hub에 올라간 이미지 쿠버네티스에 배포하기 위해 docker hub 관련 secret을 등록한다.
	  
  * cluster는 ibm public cloud(mycluster사용)  -> [ibm cloud 설치 참고](https://torchpad.com/workspace/wikis/yonwon01/pages/Kubernetes_%EC%8B%A4%EC%8A%B5%EA%B5%AC%EC%84%B1 "ibm cloud 설치 참고")
  
  * mycluster로 들어온 것을 확인 후 쿠버네티스에 docker hub 관련 secret을 등록한다. 
![스크린샷 2019-01-23 오전 10.36.03.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/nF0ihUJkSTqaH4veldOV_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.36.03.png)
> Kubectl create secret docker-registry my-secret --docker-server=hub.docker.com --docker-username=yonwon01 --docker-password=*** --docker-email=seowchoi@kr.ibm.com

## 8. 생성할 배치 yaml파일 작성후 생성한다.

![스크린샷 2019-01-23 오전 10.42.30.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/kz8HctzQi2dKCxhCk1yR_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.42.30.png)
> vi sample.yaml

![스크린샷 2019-01-23 오전 10.42.50.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Jnwjxk6MTxSJDG9tD1lW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.42.50.png)

![스크린샷 2019-01-23 오전 10.42.40.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/9vnuPdDyS4KuAuLxTE3L_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.42.40.png)
> 배치 yaml파일 생성 (kubectl create -f sample.yaml)
 
## 9. deploy 정상적으로 되었는지 확인한다.
![스크린샷 2019-01-23 오전 10.44.12.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/py1ABEySzae2XrgC86N8_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.44.12.png)

## 10. 서비스 배포할 yaml 파일 작성한다.

![스크린샷 2019-01-23 오전 10.46.41.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/l4kdL2CKTeWDYDRU8QXO_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.46.41.png)

![스크린샷 2019-01-23 오전 10.46.57.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/wz073VNTAeX5IXgWf8oq_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.46.57.png)
> kubectl apply -f sample-service.yaml

![스크린샷 2019-01-23 오후 1.22.40.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/xOf3VTDxRe4S1vRvgAkS_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.22.40.png)
> service를 통해 node port번호를 알아낸다 ( kubectl get svc )

![스크린샷 2019-01-23 오후 1.25.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/5MCE6p5jQi6pJNR13SV7_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.25.59.png)
> 외부 ip 주소를 통해 웹페이지에 접속한다 

## 11. pod에 해당하는 ip, port 입력하여 호출 확인한다.
![스크린샷 2019-01-23 오전 10.53.37.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/pbZUcES6TX2v0LUqaFMD_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.53.37.png)

![스크린샷 2019-01-23 오전 10.59.00.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/7n3sfRrFQnuY5bOa73Hk_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-23%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.59.00.png)
