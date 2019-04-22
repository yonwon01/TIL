
# Kubernetes
## Kubernetes_Pod
- 파드내의 하나이상의 컨테이너가 항상 공동 스케줄링된다.
- 같은 파드내의 컨테이너는 같은 호스트네임을 갖는다.
- 각각의 파드는 고립된 성격을 갖는다
   - process id(pid) namespace
   - network namespace
   - interprocess communication(ipc) namespace
   - unix time sharing (uts) namespace

* pod 배치 구조  

![스크린샷 2019-01-16 오전 10.50.37.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/uGJIWneVTj6oh2RQk12x_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.50.37.png)


* pod 실습

 1) pod 생성하기 
 
    ```
    kubectl create -f db-pod.yaml
    ```
   
     ![스크린샷 2019-01-16 오전 10.56.27.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/oVrlhFS6i1zapM5xr7oA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.56.27.png)  
    
      db-pod.yaml 파일
![스크린샷 2019-01-16 오전 10.55.41.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ZQMHnieGR6ij9WgUS0i8_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.55.41.png)

2) pod 생성된것 확인하기
    ```
    kubectl get pod
    ```
![스크린샷 2019-01-16 오전 11.09.11.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/HDjQeRQKRlmxowRsOZ3A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.09.11.png)
    
 3) 생성된 pod 정보 확인하기 
    ```
    kubectl describe pod "pod이름"
    ```
    
![스크린샷 2019-01-16 오전 11.13.49.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/pKhAve80Q0SeT9FfYJdc_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.13.49.png)


### Replication Controller
* Replication Controller
    * pod를 항상 사용가능한 상태로 만들어준다
    * 정해놓은 pod갯수(replicas) 만큼을 유지한다
       * 만약 pod의 수가 초과하면,RC는 초과한 수만큼의 pod를 지운다
       * pod가 실패하거나,지워지거나,죽으면 새로운 pod를 생성한다.
    * pod와 RC는 Label을 통해 연관된다.

* Replication Controller 구조
![스크린샷 2019-01-16 오전 11.20.00.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/jbQIkEFRpCBXLcSoNh0Y_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.20.00.png)

 * rc 실습
    
  1) rc 생성하기  
	
    ```
    kubectl create -f "yaml파일"
    ```
![스크린샷 2019-01-16 오전 11.23.38.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dp5NeZumT7mJee8CyxoY_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.23.38.png)

       
![스크린샷 2019-01-16 오전 11.21.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/xSAgoCgRuS27qTAUudQW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.21.36.png)
   - replica를 2로 두었다 -> pod의 수를 2개로 유지한다.
   
   2) rc 생성된것을 확인한다 
   ``` 
   kubectl get rc
   ```
![스크린샷 2019-01-16 오전 11.25.58.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/E3aDTGPTzqMw4aa1mIsZ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.25.58.png)

   3) 2개의 pod가 생성된것을 확인한다.
  ```
  kubectl get pod
  ```
![스크린샷 2019-01-16 오전 11.30.54.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Lxhzi3OoQHe32dk6LH7U_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.30.54.png)
> RC를 통해 web-4j8pb,web-p9q56가 생성되었다.

   4) replica 늘려보기 
   ```
   kubectl scale rc web --replicas=늘릴갯수
   ```
![스크린샷 2019-01-16 오전 11.33.42.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Wpvq4hsDRmytwtigJlTW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.33.42.png)
   - replica를 10개로 늘린 결과  pods의갯수가 10개가 되었다.

