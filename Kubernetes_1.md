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
	  1) pod 생성하기 (kubectl create -f db-pod.yaml)
    
     ![스크린샷 2019-01-16 오전 10.56.27.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/oVrlhFS6i1zapM5xr7oA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.56.27.png)  
    
      db-pod.yaml 파일
![스크린샷 2019-01-16 오전 10.55.41.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/ZQMHnieGR6ij9WgUS0i8_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.55.41.png)

    2) pod 생성된것 확인하기 ( kubectl get pod)
![스크린샷 2019-01-16 오전 11.09.11.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/HDjQeRQKRlmxowRsOZ3A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.09.11.png)
    
    3) 생성된 pod 정보 확인하기 ( kubectl describe pod "pod이름")
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
     	1) rc 생성하기  (kubectl create -f "yaml파일")
![스크린샷 2019-01-16 오전 11.23.38.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dp5NeZumT7mJee8CyxoY_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.23.38.png)

        yaml파일
![스크린샷 2019-01-16 오전 11.21.36.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/xSAgoCgRuS27qTAUudQW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.21.36.png)
   > replica를 2로 두었다 -> pod의 수를 2개로 유지한다.
   
   
        2) rc 생성된것을 확인한다 ( kubectl get rc)
![스크린샷 2019-01-16 오전 11.25.58.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/E3aDTGPTzqMw4aa1mIsZ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.25.58.png)

        3) 2개의 pod가 생성된것을 확인한다.(kubectl get pod)
![스크린샷 2019-01-16 오전 11.30.54.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Lxhzi3OoQHe32dk6LH7U_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.30.54.png)
> RC를 통해 web-4j8pb,web-p9q56가 생성되었다.

        4) replica 늘려보기 (kubectl scale rc web --replicas=늘릴갯수)
![스크린샷 2019-01-16 오전 11.33.42.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Wpvq4hsDRmytwtigJlTW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-16%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2011.33.42.png)
> replica를 10개로 늘린 결과  pods의갯수가 10개가 되었다.

### Authorization(권한관리)
  
  - 쿠버네티스 클러스터의 api에 접근하기 위해서는 우선 유효한 사용자 인지 인증(authentication)을 거처야 한다. 인증이 됐으면 그 사용자가 접근하려고하는 api에 권한이 있는지 확인이 된 다음에 api를 사용할 수 있다. 하나의 클러스터를 여러명의 사용자가 사용하는 경우에 api나 네임스페이스별로 권한을 구분해서 권한이 있는 곳에만 접근을 가능하게 할 수 있다. 특정 자원에 대한 읽기 전용 권한만 추가해서 다른 사람이 관리하는 네임스페이스라고 하더라도 참고용으로 살펴보게 할수도 있다. 물론 관리자는 모든 api에 대한 권한을 열어두고 모든 자원에 접근할수가 있다. 쿠버네티스에서는 이러한 권한 관리를 위해서 여러가지 방법을 제공하고 있다.
  
  - 권한관리 방법 ( 무슨 방법을 사용 할 지는 apiserver를 시작할때 옵션에 --authorization-mode 로 지정)
  
    1) ABAC(Attribute-based access control) : 속성(Attribute)기반의 권한 관리 입니다. 사용가능한 속성으로는 일반적으로 사용자(user), 그룹(group), 요청 경로(request path), 요청 동사(request verb)등외에도 네임스페이스, 자원등으로 설정할 수도 있습니다. ABAC같은 경우는 권한에 대한 내용을 파일로 관리하기 때문에 권한을 변경하려면 직접 마스터 서버에 들어가서 파일을 변경하고 apiserver를 재시작해주어야 하기 때문에 관리하기 번거로운 측면이 있습니다. 
   
    2) RBAC(Role-based access control) :  역할(Role)기반권한 관리이다. 사용자와 역할을 별개로 선언한 다음에 그 2가지를 엮어서(binding)해서 사용자에게 권한을 부여해 준다. RBAC은 관리하기 위해서 서버에 접근할 필요없이 kubectl이나 api 를 이용해서 관리하는 것이 가능
 
- RBAC(Role-based access control) 기본개념 : 롤(Role)은 특정 api나 리소스에 대한 권한들을 명시해둔 규칙들의 집합
        1. 롤  : 그 롤이 속한 네임스페이스에 한곳에만 적용
        2. 클러스터롤 : 특정 네임스페이스에 대한 권한이 아닌 클러스터 전체에 대한 권한을 관리. 추가로 네임스페이스에 한정되지 않은 자원 및 api들에 대한 권한을 지정할 수 있다. 노드들에 대한 권한이라던가 “/healthz” 같은 엔드포인트에 대한 권한도 관리
     
    ![스크린샷 2019-01-17 오후 5.02.59.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/gANlmdhcRwuNQoPvFu2j_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.02.59.png)
     - 사용자의 계정은 개개별 사용자인 user, 그리고 그 사용자들의 그룹은 user group, 마지막으로 시스템의 계정을 정의하는 service account로 정의된다.
     -  권한은 Role이라는 개념으로 정의가 되는데, 이 Role에는 각각의 리소스에 대한 권한이 정의된다. 예를 들어 pod 정보에대한 create/list/delete등을 정의할 수 있다. 
     
-
- read-role.yaml 파일
     
     ![스크린샷 2019-01-17 오후 5.09.51.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/LnY7XLSQ4i5gPwaEgQMA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.09.51.png)
 > metadata : 이 롤이 속한 네임스페이스를 명시
  name : 롤의 이름
  rules : 롤이 가진 권한 (배열형식)
  apiGroups : 이 롤이 사용할 api 그룹들을 설정
  resources : 어떤자원에 접근이 가능한지 설정
  verbs : 어떤 동작이 가능한지 설정
  resourceNames : rules에 resourceNames 를 추가하고 "mypod"를 추가하면 mypod라는 이름을 가진 pod에만 조회가 가능(resourceName이 지정되면 create, watch, list, deletecollection등의 verb는 작동하지 않는다. verb는 결국 개별 api를 호출하는 패턴을 지정하는건데 이 verb들의 api 패턴은 개별 리소스의 이름을 명시하는 형식이 아니기 때문에 적용되지 않는다)
 
 
