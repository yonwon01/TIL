##  Kubernetes
* 개념 : 애플리케이션을 배포하고 확장 및 관리를 자동화하는 컨테이너 오케스트레이션


* kubernetes 구조
     * 크게 마스터노드와 워크노드로 나뉜다. 클러스터 전체를 관리하는 컨트롤러로써 마스터가 존재하고, 컨테이너가 배포되는 머신 (가상머신이나 물리적인 서버머신)인 노드가 존재한다.
     
     ![architect_image](https://github.com/yonwon01/TIL/blob/master/image/architect_image.png)
* 오브젝트
     * 기본 오브젝트 (Basic object) :컨테이너화되어 배포되는 애플리케이션의 워크로드를 기술하는 오브젝트 (Pod,Service,Volume,Namespace)
     * 컨트롤러 (Controller) : 기본 오브젝트(Basic object) 를 생성하고 관리하는 추가적인 기능
     * 스펙(설정)이외에 추가정보인 메타 정보

*  pod 란?
     * 쿠버네티스에서 가장 기본적인 배포 단위로, 컨테이너를 포함하는 단위
     - pod 특징
          1) 1개의 Pod는 내부에 여러개의 컨테이너를 가질 수 있지만 대부분 1~2개의 컨테이너를 갖게 된다.
          2) 1개의 Pod는 여러개의 물리서버에 나눠지는 것이 아니고 1개의 물리서버(Node) 위에서 실행된다.
          3) Pod는 클러스터 내에서 사용할 수 있는 유니크한 네트워크 ip를 할당 받지만 이 ip는 서비스에서 사용하지 않는다. -> K8s에서 Pod는 언젠가는 반드시 죽는(mortal) 오브젝트이기 때문이다. -> "Service" 라는 오브젝트를 통해 접근 가능 (Service 정의부분 참고)
          4) Pod 내부의 컨테이너들은 네트워크와 볼륨 등을 공유하고 서로 localhost로 통신할 수 있다. ex)컨테이너 A가 8080, 컨테이너 B가 7001로 배포가 되었을 때, B에서 A를 호출할때는 localhost:8080 으로 호출하면 되고, 반대로 A에서 B를 호출할때에넌 localhost:7001로 호출 가능
          
*  Service 란?
     * Pod의 경우에 지정되는 Ip가 랜덤하게 지정이 되고 리스타트 때마다 변하기 때문에 고정된 엔드포인트로 호출이 어렵다. 또한 여러 Pod에 같은 애플리케이션을 운용할 경우 이 Pod 간의 로드밸런싱을 지원해줘야 하는데, 그 역할을 하는 것이 service이다.
     
   ![service_image](https://github.com/yonwon01/TIL/blob/master/image/service_image.png)

*  Volume 이란?
     * Pod에 종속되는 디스크이다. (컨테이너 단위가 아님). Pod 단위이기 때문에, 그 Pod에 속해 있는 여러개의 컨테이너가 공유해서 사용될 수 있다.
     - volume 특징
          1) 파드의 모든 컨테이너는 볼륨에 접근이 가능
          2) vloume은 pods가 삭제가 되어도 유지가능 ex) 어플리케이션의 특성에 따라서 컨테이너가 죽더라도 데이터가 사라지면 안되고 보존되어야 하는 경우가 있다. 대표적으로 정보를 파일로 기록해두는 젠킨스가 있고, mysql같은 데이터베이스도 컨테이너가 내려가거나 재시작했다고해서 데이터가 사라지면 안된다. 이럴때 volume은 컨테이너가 재시작을 하더라도 데이터가 사라지지 않고 유지한다.
          3) volume은 여러 백앤드 storage를 기반으로 함
     - volume type
        * Host-based
            * EmptyDir
            * HostPath
        * Block Storage
            * Amazon EBS
            * GCE Persistent Disk
            * Azure Disk
            * vSphere Volume
            * ...
        * Distributed File System
            * NFS
            * Ceph
            * Gluster
            * Amazon EFSs
            * Azure File System
            * ...
        * Other
            * Flocker
            * iScsi
            * Git Repo
            * Quobyte
            
     - Persistent Volumes & Claims
        * persistentVolume (PV) : 관리자가 저장장소를 네트워킹 하며 관리하는 공간. 
            * 특징 : 컨테이너가 재시작할때 데이터를 쌓아뒀던 노드가 아니라 다른 노드에서 실행된다고 하더라도 자동으로 데이터가 있는 볼륨이 컨테이너가 새로 시작한 노드에 옮겨 붙어서 쌓아뒀던 데이터를 그대로 활용해서 사용할 수 있다. -> 하나의 서버에서만 데이터를 기록해두고 사용하는것 보다 더 안정적으로 서비스를 운영

        * PersistentVolumeClaim (PVC) : 사용자가 얼마나 저장자원을 사용할것인지 정의 하고 요구하는 것 
            * 특징 : 사용자는 pvc라는 claim을 통해 persistent volume을 제공받을 수 있기때문에 실제 pv를 신경쓰지않고 volume을 사용할 수 있다. 만약 pvc의 조건을 만족하는 pv가 존재하지 않을때는 자동으로 프로비저닝 해주는 dynamic provisionin 과 이때 쓰이는 specification template 인 StorageClass라는 오브젝트가 존재한다
            
            ![pvc-image](https://github.com/yonwon01/TIL/blob/master/image/pvc-image.png)

* Namespace 이란?
        * 네임스페이스는 한 쿠버네티스 클러스터내의 논리적인 분리단위라고 보면 된다. Pod,Service 등은 네임 스페이스 별로 생성이나 관리가 될 수 있고, 사용자의 권한 역시 이 네임 스페이스 별로 나눠서 부여할 수 있다.

* 컨테이너 오케스트레이션 이란? 
    * 컨테이너와 호스트가 수십수백개가 되어 관리하기 어려운것을 자동화를 통해 배포에 최적화 하는 것
      
* 컨테이너 오케스트레이션 특징 :
    * 프로비저닝 : 컨테이너를 자동으로 배치하고 복제하며 예약하고 시작할 수 있다
    * 구성 스크립트 : 특정 어플리케이션 구성 정보를 컨테이너에 로드할 수 있다.
    * 모니터링 : 컨테이너의 상태를 감시하고, 로드밸런싱을 수행할 수 있다, 컨테이너의 장애를 탐지하고 새로운 인스턴스를 기동가능하다, 필요시 컨테이너를 추가하거나 삭제할 수 도 있다.
    * 서비스 탐색 : 사용자가 일일이 서비스를 지정하지 않고, 컨테이너가 적합한 자원을 찾기 위해 서비스 탐색을 사용하고, 외부로 노출시킨다.
    


    

      
