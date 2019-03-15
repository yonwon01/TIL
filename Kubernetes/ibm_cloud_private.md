# ibm_cloud_private_환경구축
## 목표 : public 서버에 ICP(Ibm Cloud Private)구축을 해봄으로써 kubernetes에 대한 전체흐름을 이해한다.
## 환경구성
- 총 4대의 노드로 구성된 클라우드 환경 (서버는 ibm public cloud(softlayer)사용) 

Node구성 | Ram크기 | 하드디스크 | cpu | image Disk | ip
---|---|---|---|---|---
Master | 8GB | 100GB | 2 X 2.0 core | 25GB | 169.56.107.130
Management | 16GB | 100GB | 2 X 2.0 core | 25GB | 169.56.107.132
Proxy | 4GB | 100GB | 2 X 2.0 core | 25GB | 169.56.107.137
Worker | 4GB | 100GB | 2 X 2.0 core | 25GB | 169.56.107.131

## 설치하기
### 1) master node에 로그인한다.

![스크린샷 2019-01-29 오후 5.36.56.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/5puqoB4QTWXqKFjqcuQ6_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.36.56.png)

### 2) icp 설치하기 전에 hosts파일에 사용할 노드의 ip주소를 작성한다.
- vi /etc/hosts
![스크린샷 2019-01-29 오후 5.44.37.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/1TS78sUaRA2vVBgoHs4C_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.44.37.png)

![스크린샷 2019-01-29 오후 5.46.26.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/mdFVVDYRcXDTeBBVDM1w_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.46.26.png)
> IBM Cloud(softlayer)와 같은Public Cloud상에서는 Public IP대역 대로 설치를 해야 운영이 가능하다. 가령Service를 deploy한 후 운영을 할때에도 Worker Node의IP로접근할텐데, Private대역대 라면 접근을 할 수 없다. Private Network대역 대로 사용 하고자 한다면 http proxy와 별도의 network설정이 필요하다.
즉,외부와 통신해야하는 proxy node와 master node는 pubclic ip(169.xx.xx.x)로 호스트를 설정한다.

* master 노드의 public/private ip주소 (eth0 : private, eht1 : public) 
   * ifconfig -a
   
![스크린샷 2019-01-29 오후 5.56.01.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/v72tGQtPSyK70SldIiL9_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.56.01.png)

### 3) docker hub에서 ibm private cloud를 설치할 구성파일과 저장장소를 말아농은 파일(icp-inception:2.1.0.3 )을 다운받는다
 - sudo docker pull ibmcom/icp-inception:2.1.0.3 ( icp-inception:3.1.0 최신버전을 다운받으려 했지만 마스터노드의 메모리 이슈때문에 이전버전으로 설정한다)
 
![스크린샷 2019-01-29 오후 5.43.02.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zq9E09tSZKEmEz9Pvd3A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.43.02.png)

### 4) ICP 구성파일을 저장하고 설치 디렉토리를 구성한다
* sudo mkdir /opt/ibm-cloud-private-ce-2.1.0.3
* cd /opt/ibm-cloud-private-ce-2.1.0.3

![스크린샷 2019-01-30 오전 10.55.37.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/zfxDItkWT7GSYjpyy8h8_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.55.37.png)

![스크린샷 2019-01-30 오전 10.56.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/CNApzP7TpekdbU2dQ7iV_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.56.36.png)

### 5) ICP 구성파일의 압축을 푼다
*    sudo docker run -e LICENSE=accept \
   -v "$(pwd)":/data ibmcom/icp-inception:2.1.0.3 cp -r cluster /data
   
![스크린샷 2019-01-30 오전 11.01.17.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/fSWLcLZkTxumG0zOZWPL_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-30%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.01.17.png)

### 6) 각노드에 ssh_key 공유하기
* 마스터노드에서 각각의 노드에 자동으로 접근할때 계정로그인이 되어잇지않으면 인터럽트가 날것이다. 자동으로 접근 할 수있도록 만들어진 자동화 도구가 ansible이라는 것이있고 icp에서도 ansible기반이다. 즉,ssh키를 생성하여 각노드들이 공유 할 수 있도록 한다. (ansible이란)

 i) ssh키를 생성한다
 
 ```
   ssh-keygen -b 4096 -f ~/.ssh/id_rsa -N ""
 ```
   
 ii) 인증키를 각 노드에 추가한다

   ```
       ssh-copy-id -i ~/.ssh/id_rsa.pub <user>@<node_ip_address>
   ```     
   
 iii) /<installation_directory>/cluster 폴더의 ssh_key파일에 만들어놓은 ssh키를 복사한다.
 
   ```
       sudo cp ~/.ssh/id_rsa ./cluster/ssh_key
   ```
 
### 7) 마스터노드에서 전체 노드에 환경배포하기
 
   






