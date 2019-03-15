# Microservice_Devops
### 목표 : Microservice형태의 SpringBoot 기반 Application 을 ICP환경으로 만들고 DevOps를 구축한다.
## 환경

 * 구성노드
 
Node구성 | Ram크기 | cpu | ip
---|---|---|---|---|---
Master,Etcd | 32GB | 8 core | 10.10.80.11
Proxy | 8GB | 4 core | 10.10.89.12
Management | 16GB | 8 core | 10.10.89.13
VA | 16GB | 8 core | 10.10.89.14
Worker1 | 16GB | 4 core | 10.10.89.14
Worker2 | 16GB | 4 core | 10.10.89.15
Worker3 | 16GB | 4 core | 10.10.89.16
Worker4 | 16GB | 4 core | 10.10.89.17

* Application : Microservice예제인 petClinic 를 커스터마이징 함.
[customized_petClinic](https://github.com/yonwon01/petclinic-java "customized_petClinic_github_url") -> 총4개의 서비스로분리(API gateway, customer service, vet service, visits service)

 ![스크린샷 2019-02-27 오후 10.25.14.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/bDHhW49JRKKwPODp3fhA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-02-27%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.25.14.png)


## 시나리오

![스크린샷 2019-02-27 오후 10.28.55.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/DOfOyHOdRsuMdik8NM2n_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-02-27%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.28.55.png)

1. ICP에 내장되있는 HELM 차트로 Jenkins, SonarQube 설치한다.
2. local에서 개발한 총 4가지 종류의 파일들(어플리케이션 소스코드, 이미지 레지스트리에 푸쉬할 Dockerfile, 쿠버네티스를 통해 컨테이너에 배포한 yaml, jenkins pipeline을 작성한 script파일)를 Github에 올린다.
3. Jenkins에서 Jenkins pipeline을 작성한 script파일(Jenkinsfile)을 빌드한다.
4. Jenkins에서 소스코드 빌드 후 sonarqube를 통해 코드 정적 분석한다.
5. 생성한 Dockerfile을 빌드 후 icp에 내장된 private docker registry 에 push한다.
6. 만들어놓은 yaml파일을 kubectl 를 통해 pod,service 형태로 배포한다.
7. slack을 통해 성공,실패 알람을 확인한다.




