> 공부중에 작성한 내용입니다. 정확하지 않을 수 있어요. 😀

목차
---
 Docker-compose 개념/ 구조
   * [Docker Compose란?](#docker-compose란?)
   * [YAML(야멜) 문법](#yaml(야멜)-문법)
   * [Docker Compose 사용법](#docker-compose-사용법)
        * [version](#version)
        * [services](#servies)
        * [volumes](#volumes)
   * [Docker Compose 실행/중지하기](#docker-compose-실행/중지하기)
   * [그 외 명령들](#그-외-명령들)
---
## Docker Compose란?
* 보통 도커에서는, 이미지 하나당 하나의 컨테이너만 만들어서 관리를 하는데, 이러한 여러 개의 컨테이너를 연결하여 동시에 관리하는 툴이다.
* 웹 서비스는 프론트엔드 서버, 백엔드 서버, 데이터베이스 서버로 이루어져 있는 경우가 많은데, 이 각각의 서버를 컨테이너로 만들고 연결하여 서비스를 한다. 
이때 Docker Compose가 사용되고, 나아가 쿠버네티스 등의 관리 툴을 배우고 관리할 때에도 수월할 수 있다.

## YAML(야멜) 문법
* Docker Compose는 .yaml 확장자의 파일을 작성하여 실행을 한다. 
* 또다른 데이터 구조화 문법인 JSON과 같이 Key, Value로 이루어져 있다. 하지만, JSON과는 다르게 들여쓰기로 리스트를 구분한다.


* 기본문법 
  * #: 해당 라인을 주석처리
  * ---: 문서 시작을 나타냄(옵션)
  * ...: 문서의 끝을 나타냄(옵션)
  * 만약 데이터의 값이 리스트의 형태라면(JSON에서의 [ ]) 들여쓰기를 한 뒤, 하이픈(-)을 붙인다.
  
## Docker Compose 사용법
  기본적으로, Docker compose는 4가지의 카테고리(service, version, volumns, networks)로 작성하며, 특히 service와 version을 많이 사용한다.
  ### version
  어떤 버전의 docker compose를 사용할 것인지 지정해준다.
  보통 "3"을 많이 사용한다.
  ![](https://images.velog.io/images/tjdwns2243/post/ad0364e5-b64f-4de3-888c-fc68cc86f6f6/image.png)
  
  ### services
  앞서 설명했듯, Docker Compose는 여러 개의 도커들을 관리하는 기술이다. servies 옵션은 어떠한 컨테이너를 관리할 것인지를 지정해준다.
  ![](https://images.velog.io/images/tjdwns2243/post/e7d40164-836d-4e66-8589-663ccc419102/image.png)
  위의 코드에서는, nginxproxy, nginx, apache라는 세 개의 컨테이너를 쓸 것이다.
  ### volumes
  * docker run에서의 -v옵션과 동일한 역할을 한다.
  * 호스트PC의 폴더와 컨테이너 내부 폴더를 바인딩하는 것.
  * 여러 volume을 지정할 수 있기 때문에, 리스트 형태로 작성을 해준다. 따라서, 앞에 하이픈(-)을 붙여야 한다.![](https://images.velog.io/images/tjdwns2243/post/c8f8771c-8956-4374-b509-f1b6ac428d0d/image.png)
  ### networks
  * 컨테이너간 네트워크의 분리를 위한 추가 설정 부분
  * 하지만 대형 서비스가 아닌 이상, 보통 하나의 네트워크 안에서 여러 개의 도커를 쓰기 때문에, 공부하는 입장에서 보통은 쓰지 않는다.
  ### ports
  * docker run 에서의 -p와 동일한 역할
  * YAML문법에서는 -p문법을 썼을 때와 같이 11:22와 같이 작성할 경우, 시간으로 인식을 할 수 있기 때문에, "11:22"와 같이 쌍따옴표로 감싸준다.
  ![](https://images.velog.io/images/tjdwns2243/post/6a2b4019-0ba1-40b0-b38b-042aaeb00ae9/image.png)
  
  ### restart
  * 컨테이너가 다운되었을 시, 항상 재시작하라는 설정. 서버도 사람이 만든건지라 갑자기 서버가 다운되는 변수가 일어나는데, 이 설정을 해준다면 웬만한 상황에서는 다시 복구를 자동으로 해준다(고 합니다 ㅎ)
  
  ### environment
  * Dockerfile의 ENV 옵션과 동일한 역할을 한다.
  * 옵션에 들어갈 변수 역시 리스트형태로 앞에 하이픈(-)이 붙어야 한다.
  ![](https://images.velog.io/images/tjdwns2243/post/8c0df328-de42-4886-9529-4fb9ec512098/image.png)
  * 다만, 보안을 위해서 environment에 들어갈 내용을 따로 파일로 작성하여, env_file 옵션으로 불러들이는 경우도 있다.
  ![](https://images.velog.io/images/tjdwns2243/post/c6636934-80dd-448b-92c6-a9743bc0505b/image.png)
  
  ### depends_on
  * 여러 컨테이너를 Docker Compose로 실행할 경우, 각 컨테이너가 실행을 시작하는 시점이 미묘하게 다를 수 있다.
  따라서 특정 컨테이너가 시작되자마자, 다른 컨테이너에 접속하도록 프로그래밍을 한다면, 접속하고자 하는 컨테이너가 생성되기 전이라서 의도치 않게 오류가 발생할 수 있다.
  * 따라서, 컨테이너의 depends_on 옵션에 컨테이너를 리스트형태로 적어주면, 해당 컨테이너가 생성되기 이전에, 어떤 컨테이너가 먼저 생성되어야 하는지를 도커에게 알려줄 수 있다.
  ![](https://images.velog.io/images/tjdwns2243/post/aa556aa8-4cf2-400f-a5d5-e7386d6069b7/image.png)
  ### links
  * 컨테이너 내부에서, 다른 컨테이너에 접속하고 싶을 때 사용하는 옵션.
  ![](https://images.velog.io/images/tjdwns2243/post/cfb26402-85ae-461a-bb59-5bb96b24f2cd/image.png)
  ### build
  * DockerHub에서 다운로드받은 이미지가 아니라, dockerfile을 기반으로 컨테이너를 작성하고 싶을 때 사용.
  ![](https://images.velog.io/images/tjdwns2243/post/4d361d1f-37a3-46ae-8dd8-3eab5e6069d9/image.png)
    * context: dockerfile이 있는 폴더의 위치
    * dockerfile : 사용할 도커파일명
 ### container_name
 * 컨테이너 이름 설정
 * 이 옵션 없이 그냥 docker compose로 컨테이너를 띄웠을 때, 도커파일과 관련이 있는 이름으로 도커가 그냥 임의로 지정을 해준다.
 그거 말고, 내가 직접 이름을 지어주고 싶을 때 사용.
 ![](https://images.velog.io/images/tjdwns2243/post/76084d1c-9c24-4b98-bc42-b4839b5048e0/image.png)
 
### docker
## Docker Compose 실행/중지하기
* .yml 확장자의 파일이 있는 해당 파일로 이동한뒤
  ```
  docker-compose up -d
  (-d는 백그라운드에서 실행한다는 옵션. 만약 이 옵션이 없다면,
  Docker의 출력물만 나오면서, 다른 명령은 실행할 수 없게 된다.)
  ```
  명령을 실행해준다. (하나의 폴더 안에 하나의 yml 파일만 있어야 한다. 명령이 실행되는 폴더 안에 있는 yml파일만을 참조하기 때문에, 여러 개의 docker-compose를 만들고 싶다면 여러개의 폴더를 만들고 하나의 폴더 안에 하나의 yml 파일만이 있게 해주자.)

* 만약 이미지 재빌드가 필요하다면
  ```
  docker-compose up --build -d
  ```
  와 같이 --build 옵션을 붙여준다. 만약 안붙여주면 기존에 작성되었던 이미지를 사용한다.
* Docker Compose 중지 명령
  ```
  docker compose stop
  ```
* docker-compose up 으로 만들어진 컨테이너 삭제
  ```
  docker-compose down
  ```
  
## 그 외 명령들
* docker-compose logs
각 컨테이너의 모든 로그(출력결과) 확인

* doker-compose config
실행 중인 Docker Compose 의 docker-compose.yml 설정 확인

* docker-compose exec <컨테이너이름>
실행중인 컨테이너에 명령어를 실행/ 하지만 보통 그냥 docker exec를 쓰지, 이렇게는 별로 안쓴다.
    



  
  