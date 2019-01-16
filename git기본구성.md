# git
### git 이란?
- 깃허브(GitHub)는 분산 버전 관리 툴인 깃(Git)을 사용하는 프로젝트를 지원하는 웹호스팅 서비스이다. 2011년의 조사에서는 가장 인기있는 오픈 소스 코드 저장소로 꼽혔다.

**Git 디렉토리 만들기** 

![스크린샷 2019-01-15 오후 4.30.32.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/8UDGR7sYQ5KJt4ANXrKr_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.30.32.png)
> git을 실행시켜볼 임의 디렉토리(gitDir)를 생성하고 git init 을 통해 git 디렉토리를 생성한다.

![스크린샷 2019-01-15 오후 4.33.10.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Mb7H9czKRUuFjSPeXBdH_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.33.10.png)
> .git이라는 데이터 저장장소가 만들어진 것을 확인 할 수 있다.

 **git username 설정 하기**

![스크린샷 2019-01-15 오후 4.56.16.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/tRdNU5z2R5CWcfFtlX3L_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.56.16.png)

![스크린샷 2019-01-15 오후 4.56.04.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/Ir5C3T9R01JlNxwKzkgs_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.56.04.png)
> git config --list 를 통해 환경설정 확인

**Git branch 만들기**
  * git checkout -b "브렌치이름" 
  
![스크린샷 2019-01-15 오후 4.35.33.png](https://s3-ap-northeast-1.amazonaws.com/torchpad-production/wikis/10853/HESOgBaMQSKzZUCBgjqZ_%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202019-01-15%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.35.33.png)
 > branch를 생성한 후 디렉토리를 브렌치폴더로 변경한다.

**merge**
	- branch는 독립적자원이기 때문에 파일생성시 master에서는 확인이 불가하다.이때 master에다가 branch에서 생성한 파일을 합치고 싶으면 merge를 이용한다. merge할때는 부모 브렌치에 이동해서 자식 브렌치를 가져오는 것이 일반적이다.
  - git merge develope 
   


