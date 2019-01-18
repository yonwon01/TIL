
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
