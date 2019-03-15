# Kubernetes_Storage
*  Volume 이란?
 * Pod에 종속되는 디스크이다. (컨테이너 단위가 아님). Pod 단위이기 때문에, 그 Pod에 속해 있는 여러개의 컨테이너가 공유해서 사용될 수 있다.
      volume 특징
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

