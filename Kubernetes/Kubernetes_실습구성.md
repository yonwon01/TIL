# Kubernetes_실습구성

* 목표 : Kubernetes를 이해하고 환경 설치한다
* 개발 환경 : ibm public cloud(Kubernetes) 
* 순서
    * 쿠버네티스 환경 설치
    * 어플리케이션 배포  
    
       
### 1)ibm cloud(https://www.ibm.com/cloud/) 에서 kubernetes 설치 
  
![kubernetes_create2.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/S0fr6gGHTymN5VJ1ygDW_kubernetes_create2.png)
 

![kuberntes-create1.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/iCWGobOLRysgbzZZR3pq_kuberntes-create1.png)
* 한달 Free Type 사용 (cluster 이름 : mycluster로 적용)

### 2) 로컬에서 ibm cloud 로그인 하기
 1) IBM Cloud 계정에 로그인
- ibmcloud login -a https://api.au-syd.bluemix.net
연합 ID가 있을 경우 ibmcloud login --sso를 사용하여 IBM Cloud CLI에 로그인해야 함
    
![스크린샷 2019-01-15 오전 10.04.12.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dR4jFYr8Qzah5lJvAUpt_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.04.12.png)
> 연합 id이므로 --sso를 통해 임시패스워드를 통해 로그인한 결과
 
 2) 명령을 가져와서 환경 변수를 설정하고 Kubernetes 구성 파일을 다운로드
 - ibmcloud cs cluster-config [클러스터이름]
 
![스크린샷 2019-01-15 오전 10.10.02.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/q2r3JIZKSu2wE9Ua80Da_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.10.02.png)
> 환경변수 설정을 통해 export 실행

### 3) local에 cluster 설정
1) kubectl 을 통해 사용할 클러스터를 선택한다.(kubectl 설치하기 : https://kubernetes.io/docs/tasks/tools/install-kubectl/ )
- kubectl config use-context "클러스터이름"

![스크린샷 2019-01-15 오전 10.13.56.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/9iT0SnS9moKUZ8vqX8dQ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.13.56.png)

2) 선택된 클러스터 확인 
- kubectl config get-contexts

![스크린샷 2019-01-15 오전 10.14.04.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/9OSn4lXJSMqLyloVHywy_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.14.04.png)


### 4) 간단 실습 해보기

* nginx이미지 배포해보기 
* kubectl run my-web --image=nginx --port=80
![nginx이미지 만들기_2.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/H3QOKnVhTBGzNzIL7tvU_nginx%E1%84%8B%E1%85%B5%E1%84%86%E1%85%B5%E1%84%8C%E1%85%B5%20%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5_2.png)
> deployment 생성을 통해 replica와 pod가 생성된다.

* kubectl get deployment
![이미지 올린거 배치 확인 _3.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/RI7UQALeRuGIXukKUYjB_%E1%84%8B%E1%85%B5%E1%84%86%E1%85%B5%E1%84%8C%E1%85%B5%20%E1%84%8B%E1%85%A9%E1%86%AF%E1%84%85%E1%85%B5%E1%86%AB%E1%84%80%E1%85%A5%20%E1%84%87%E1%85%A2%E1%84%8E%E1%85%B5%20%E1%84%92%E1%85%AA%E1%86%A8%E1%84%8B%E1%85%B5%E1%86%AB%20_3.png)
> my-web 배치가 올라온것을 볼 수 있다.

* kubectl expose deployment kubernetes-bootcamp --target-port=8080 --type=NodePort
![expose _5.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/mMssjBR4TwqNWTS3vnf1_expose%20_5.png)
> service 생성

* kubectl get svc 
![서비스 정보_6.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/l9vVc2VzRCi4uCaF67I3_%E1%84%89%E1%85%A5%E1%84%87%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%87%E1%85%A9_6.png)
> 생성된 정보를 확인할 수 있다.
CLUSTER-IP : 클러스터 아이피 (kubernetes에서 생성하는 아이피)
EXTERNAL-IP : <nodes>의 의미는 노드의 아이피이다. 즉, 물리 머신의 아이피
PORT : <내부포트>:<NODE_PORT>/TCP

* kubectl describe svc [서비스이름]
![myweb 서비스 설명_7.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/GN501yTuTss5eTWyj937_myweb%20%E1%84%89%E1%85%A5%E1%84%87%E1%85%B5%E1%84%89%E1%85%B3%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%86%E1%85%A7%E1%86%BC_7.png)
> describe명령어를 통해 서비스 설명

* 대쉬보드 확인
![kubernetes 대쉬보드.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/AGkrQcenTOzx6SiJGpUm_kubernetes%20%E1%84%83%E1%85%A2%E1%84%89%E1%85%B1%E1%84%87%E1%85%A9%E1%84%83%E1%85%B3.png)ㅇ
> ibm cloud 대쉬보드를 통해 my-web pod와 서비스가 올라온것을 확인 할 수 있다.

* nginx 실행 
![public_cloud_nginx완성.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/C0fb3vmDT3e3I1tVnj51_public_cloud_nginx%E1%84%8B%E1%85%AA%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC.png)
> [public ip:노드포트] 를 통해 nginx가 실행 되는것을 확인 할 수 있다.
