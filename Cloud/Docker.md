# Docker
#### Docker 실습해보기
#### - Docker는 리눅스 기반 OS에서 실행이 되며 이번 실습은 MAC os 에서 실행시켰습니다.

##### 1. docker version 확인 -([docker 설치하기](https://docs.docker.com/install/ "docker 설치하기") )
![스크린샷 2019-03-13 오후 5.27.35.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/1p8zAOsKQV6I3b9Y5hYu_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.27.35.png)

> docker version

##### 2. docker hub에서 image가저오기
- docker hub은 docker image를 저장하는 public registry 로서 모든 유저들이 사용할 수 있도록 공유한다. 생성한 image를 개인적으로 보관하고 싶으면 private registry 공간을 만들어서 이미지를 가져올 수 있다.

![스크린샷 2019-03-13 오후 5.37.44.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/MxNifOiQWSDXFjKs4Ef6_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.37.44.png)

> docker image pull image이름 
(private registry 인 경우 pull 다음 저장소 이름을 입력)

##### 3. 가저온 이미지를 컨테이너에 띄우고 컨테이너에 접속한다.(이미지 파일 자체는 파일시스템으로 아무 역할을 하지 못한다.container에 run 시키는 순간 부터 실행할 수 있다.)
![스크린샷 2019-03-13 오후 1.43.19.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/k66d9jugQtSqBXFqswAL_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.43.19.png)

> docker container run -it alpine /bin/sh
- it 는 interactive tty 로서 컨테이너와 터미널을 연결시켜 호스트에서 접속하기위해 필요한 option이다.

#### 4. container 를 run 명령어로 실행시키면 나가는 순간 exit 된다.
즉, docker container run -it --name myalpine alpine /bin/ash 에서 exit 하면 status는 exited 된다. 백그라운드로 계속 실행 시키기 위해서는 -d 옵션을 주어야 한다.

![스크린샷 2019-03-13 오후 1.52.01.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/RE09vZaxQ26rz8nAx5fv_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.52.01.png)

> docker container ls  : 실행중인 컨테이너 보기
docker container ls -al : 모든 컨테이너 보기
docker container run -it -d alpine /bin/ash : 컨테이너 쉘에서 작업이 끝나도 계속 죽지않고 백그라운드에서 돌아간다.

#### 5. 컨테이너의 메인프로세스 이외에 서브프로세스 실행시키기 - docker run 명령어를 통해 컨테이너를 띄어놓은 상태에서 exec명령어로 서브프로레스를 실행시킬 수 있다. 예를들어 sh로 쉘을 띄어 보고 실행되고 있는 프로세스를 확인 해보겠다.

![스크린샷 2019-03-14 오전 10.08.38.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/y904LFb5Romp3jMighQ2_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-14%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.08.38.png)

> docker container run -it -d --name myalpine alpine /bin/sh : container 를 실행시키고 백그라운드로 돌아가도록 해놓기 
docker container ls : 실행중인 컨테이너 확인하기
docker container exec -it <containerID> sh : 실행중인 컨테이너에 sh 로 접속하기
ps auxf : 실행중인 프로세스 확인하기 
- 확인결과 메인 프로세스인 /bin/sh 이외에도 exec로 띄운 sh도 돌아가고 있는 것을 확인할 수 있다.

#### 6. 컨테이너 삭제하기 - 컨테이너를 삭제하기 위해서는 실행을 중지시킨 후에 삭제 할 수 있다.

![스크린샷 2019-03-14 오전 10.43.30.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/4MMxmZEJRQq5RYTQZ3xn_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-14%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.43.30.png)

> docker container stop <containerID> : container 실행 중단하기
docker container rm <containerID> : container 삭제하기
docker container prune : 실행중이지 않은 컨테이너 모두 삭제하기

* 컨테이너 삭제시 한번에 여러 컨테이너를 삭제하는 법 : 띄어져 있는 컨테이너들의 ID 를 변수명으로 만들어서 삭제한다.

![스크린샷 2019-03-14 오전 10.52.09.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/LHd1gLWGSa1SaazFvC3A_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-14%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.52.09.png)
> docker container ls -a -q : -q 옵션은 생성되있는 컨테이너의 ID 만 출력한다.
docker container stop $(docker container ls -a -q) : $() 는 변수명으로 변환하는 명령어로 생성되있는 컨테이너 ID를 변수로 지정한다.
docker container rm $(docker container ls -a -q) : 생성된 모든 컨테이너를 삭제한다.

#### 7. 외부에서 접속하기 - 외부에서 접속 하기 위해서는 포트번호를 호스트에 바인딩 해주어야 한다.


![스크린샷 2019-03-14 오후 1.31.17.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/s0yxgXSbQviYqeBlJiHP_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.31.17.png)
> docker container run -d --name <컨테이너이름> -p 8080:80 <이미지파일>:<태그>  - -p 옵션을 통해 포트번호를 바인딩한다.
![스크린샷 2019-03-14 오후 1.37.33.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/yIL6j8PuS7mVytZRvWJC_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%201.37.33.png)
> ifconfig en0 - macOS에서 호스트 public ip검색(리눅스는 ifconfig eth1)
![스크린샷 2019-03-13 오후 2.50.48.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/mPvWfo6KTGmBIEK9f6lN_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-03-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.50.48.png)
> container의 80포트번호를 호스트의 8080포트로 바인딩하여 public ip를 통해 외부에서도 접속 할 수 있도록 한다.

#### 8. container의 설정파일을 호스트에서 다루기
