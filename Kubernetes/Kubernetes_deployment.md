# Kubernetes_deployment
## Deployment
	
  * deployment 란?
   - deployment는 pod와 replica set으로 배포할때 수동적으로 갯수를 조정해야하는 점을 자동으로 할수 있도록 만든 추상화된 개념이다 
   - 어플리케이션을 관리하고 스케일링 하는데 유동적이다

  * deployment 방식
   
       * 블루/그린  :  기존에 띄워져 있는 파드 개수와 동일한 개수 만큼의 신규파드를 모두 띄운 다음에 신규 포드가 이상없이 정상적으로 떴는지 확인한 다음에 들어오는 트래픽을 한번에 신규포드쪽으로 옮기는 방법 
       * 롤링 업데이트 : 새 버전을 배포하면서, 새 버전 인스턴스를 하나씩 늘려나가고, 기존 버전을 하나씩 줄여나가는 방식 
       * 카날리 배포 : 신규 버전을 배포할 때 한꺼번에 앱의 전체를 교체하는게 아니라 기존 버전을 유지한 채로 일부 버전만 신규 버전으로 올려서 신규 버전에 버그나 이상은 없는지를 사용자 반응은 어떤지 확인하는데 유용하게 사용하는 방법 (쿠버네티스의 기본 디플로이먼트로는 디플로이먼트에 속한 포드들을 하나씩이든 한꺼번에든 모두 교체하는 방식이기 때문에 이런 카나리 배포를 하기에는 어려움이 있다. 하지만 라벨을 이용하면 쿠버네티스에서도 카나리 배포를 할 수 있다.)

## rolling update 방식 시나리오 

![rc_1.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Ta06HCTRazb7t1QDLzTg_rc_1.png)
> v2를 배포하기 위해서, v2 Pod를 컨트롤한 RC를 만들고 replica의 수를 1로해서 v2 Pod를 하나 생성한다. 그리고 RC v1에서, replica의 수를 3 → 2로 줄여서 v1 Pod의 수를 2개로 조정한다.

![rc_2.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/7w9TF8cTcKu2clMOJ1qB_rc_2.png)
> v1 Pod의 수를 1로 조절하고, v2 Pod의 수를 2로 늘린다. 마지막으로, Pod v1을 0으로 줄이고, RC v1을 삭제한다. 그리고 Pod v2을 하나 더 늘린다.



![스크린샷 2019-01-14 오후 5.46.48.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/kXcfwmJPTkyQZexIie6I_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.46.48.png)
> j-hello.yaml 로 Deployment를 정의한 내용

![스크린샷 2019-01-14 오후 5.48.28.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/dxNs6KPdSKyPvvNgtDmW_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.48.28.png)
> j-hello-svc.yaml 로 서비스 배포파일 정의

![스크린샷 2019-01-14 오후 5.54.53.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/JlUPiX6IS7ydM6XmrMjS_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.54.53.png)
> 생성된 pod 3개를 볼 수 있다. 실행 시켜 본 결과 hello world! 가 출력되는 것을 알 수 있다.

![스크린샷 2019-01-14 오후 5.57.10.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/QEMv5L3Tm6K9nw9QGHg6_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.57.10.png)
> 생성된 서비스를 볼 수 있다.

![스크린샷 2019-01-14 오후 6.00.49.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/RH8Qey5QRrmToovKbjfs_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.00.49.png)
> scale up을 통해 replica 3개에서 10개로 늘린다.

![스크린샷 2019-01-14 오후 6.03.33.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/5627PXtTTT2DY3ofeiaT_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.03.33.png)
> v2이미지에 docker이미지 업데이트를 한다.


![스크린샷 2019-01-14 오후 6.17.08.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/xEXykgcR0aSkVdCtECwA_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.17.08.png)

![스크린샷 2019-01-14 오후 6.16.52.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/WbctyOUPTJyEL9gPF817_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.16.52.png)
> 업데이트를 통해 이미지가 변경되고 있는 것을 볼 수 있다.

![스크린샷 2019-01-14 오후 6.29.02.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/PQXysNKTFGRHJmsAbEun_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.29.02.png)
> 업데이트가 끝나고 pod가 모두 변경된 것을 볼 수 있다. 확인해보면 위처럼 Pod의 이름이 j-hello-79cf76d49d-* 으로, 새로 배포된 RS 의 이름과 동일한것을 볼 수 있다.


#### rollout,rollback

![스크린샷 2019-01-14 오후 6.39.43.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/D1hjUs2LT9GMKEAASVVJ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.39.43.png)
> kubectl rollout history deployment/[Deployment 이름]을 실행하면 기존에 배포된 버전을 확인할 수 있다. 디폴트로는 2개의 버전을 유지하게 되어 있다.

![스크린샷 2019-01-14 오후 6.39.49.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/R3nt3JP3RFmvJWrQvCWH_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.39.49.png)
> 1버전으로 롤백을 하려면 kubectl rollout undo deployment [ deployment 명 ] --to-revision=[롤백할 버전명] 을 하면, 그 버전으로 롤백이 된다.





