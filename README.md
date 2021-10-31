# Docker 학습정리
> Docker Study Note

<br>

# 목차
- [Command](#1)
    - [Basic Command](#2)


- [레퍼런스 링크](#99)
    - [초보를 위한 도커 안내서 - 이미지 만들고 배포하기 SERIES 3/3](https://subicura.com/2017/02/10/docker-guide-for-beginners-create-image-and-deploy.html)
    - [도커 명령어 정리](https://nirsa.tistory.com/66?category=868315)
    - [공식 docker-compose 레퍼런스](https://docs.docker.com/compose/reference/)
    - [Docker Compose 커맨드 사용법](https://www.daleseo.com/docker-compose/)
    - [YAML 문법](https://subicura.com/k8s/prepare/yaml.html#%E1%84%80%E1%85%B5%E1%84%87%E1%85%A9%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AB%E1%84%87%E1%85%A5%E1%86%B8)
    - [YAML 문법 2](https://lejewk.github.io/yaml-syntax/)
    - [JSON to YAML](https://www.json2yaml.com/)
    

<br>

<a name="1"></a>

# Command

## 도커 환경확인
``` docker
# 도커버전 확인
$ docker version

# 도커 시스템 정보 확인
$ docker system info
```

## 이미지 다운로드
```bash
docker pull [이미지 이름]
```

## 이미지 목록확인
```bash
docker image ls
```

## 실행중인 도커 컨테이너 확인 시
```bash
docker ps       # 실행중인 도커만 확인 시
docker ps -a    # 멈춰진 컨테이너 포함해서 확인
```

## 컨테이너 실행
```bash
# ubuntu의 latest 태그를 --it 옵션과 함꼐 run 실행시킨다.
# 기존에 저장된 image가 없는 경우 자동으로 이미지를 pull 한 이후 run 실행
docker container run --interative --tty --name ubuntu ubuntu:latest
docker container run --it --name ubuntu ubuntu:latest
```
> --interactive, -i 표준 입력창을 엽니다. 

> --tty, -t 장치에 tty를 할당합니다.

> --name 컨테이너 이름 설정

<br>

## 백그라운드 컨테이너 실행
[[Docker] 도커에서 Container 포트와 Host 포트의 개념](https://m.blog.naver.com/alice_k106/220278762795)
```bash
# ref : 멋쟁이사자
# 컨테이너를 "백그라운드"에서 실행하고 "호스트의 80번 포트를 컨테이너의 80번 포트로 포워딩" 한다. 
docker container run -d -p 80:80 --name apache httpd:latest
```
> --detach, -d 백그라운드에서 컨테이너를 실행

> --publish, -p 호스트:컨테이너 포트포워딩을 세팅


<br>

## 컨테이너 중지
`stop` 명령어
```bash
docker container stop [컨테이너 명]
```

## 컨테이너 삭제
`rm` 명령어
```bash
docker rm [컨테이너 명]
```


<br>

## 컨테이너 재시작
`restart` 명령어
```bash
docker container restart [컨테이너 명]
```

<br>

## 컨테이너 구동 확인
`stats` 명령어
```bash
docker container stats [컨테이너 명]
```

<br>

## 컨테이너 목록확인
`ls` 명령어
```bash
docker container ls [컨테이너 명]
```


<br>

## 구동중인 컨테이너 연결
`attach` 명령어
```bash
docker container attach [컨테이너 명]
```

<br>

## 구동중인 컨테이너에서 커맨드 실행
`exec` 명령어
```bash
# 예시
docker container exec -it ubuntu /bin/echo "Hello, Docker!"
```

<br>

## 컨테이너 내부에서 구동 중인 프로세스 확인
`top` 명령어
```bash
docker container top [컨테이너 명]
```

<br>

## 컨테이너 이름변경
`rename` 명령어
```bash
docker rename [기존 이름] [신규 이름]
```

<br>

## 컨테이너 변경사항 확인
`diff` 명령어
```bash
docker diff [컨테이너 명]
```

<br>

## 유휴(IDLE) 이미지 및 컨테이너 삭제
```bash
# 이미지 제거
docker image rm [OPTIONS] IMAGE [IMAGE...]

# 실행하지 않은 컨테이너의 이미지만 삭제 (컨테이너가 실행중이라면 삭제되지 않는다)
docker image prune

# 중지된 커테이너까지 확인 할 수 있다.
docker ps -a

# 실행하지 않은 컨테이너를 삭제한다
docker container prune
```

## Docker Push
docker hub에 빌드 이미지 업로드
```bash
# 먼저 docker loing을 하여 로그인을 한다.
docker login
docker push {이미지이름}:{태그명}
```

## Docker Save Image
```bash
# tar확장자 권장
docker image save -o {파일이름} {이미지이름:태그명}
> docker image -o react-app.tar react-app:3
```

## Docker Image Build
```bash
docker build -t [image_name]:[tag_name] .
# -t 는 태그를 의미한다
# 'hello-docker' 라는 이름으로 빌드할것이며 
# . 경로(현재경로) 기준으로 빌드한다는 의미
```

## Docker Image load
```bash
docker image load -i react-app.tar
```

## Docker Add tag
```bash
# 실행 후 docker ls 출력 시 동일한 이미지에 다른 태그이름 생성확인가능
docker image tag {이미지}:{태그이름}  {경로}
```

## Docker 파일복사
```bash
docker cp {컨테이너이름}:{복사할 컨테이너내부 대상} {localhost 경로}
```

## Docker Volume
Sharing the Source Code with a Container
### ref : https://0902.tistory.com/6
```bash
# Volume Create
docker volume create [name]

# inspect volume
docker volume inspect [volume name]

# container1의 /data 디렉토리와 container2의 /data 디렉토리를 호스트의 /root/data 디렉토리와 매핑
docker run -it --name container1 -v /root/data:/data [이미지] [커맨드]
docker run -it --name container1 -v /root/data:/data centos /bin/bash
```

<br>

# Docker Compose

`up` 명령어 : Docker Compose 컨테이너 생성 및 실행
```bash
docker-compose up
```

`down` 명령어 : Docker Compose 컨테이너 정지 및 삭제
```bash
docker-compose down
```

`start` 명령어 : 중지된 Docker Compose 컨테이너 시작
```bash
docker-compose down
```

`stop` 명령어 : Docker Compose 컨테이너 정지
```bash
docker-compose down
```

`ps` 명령어 : Docker Compose 컨테이너 조회
```bash
docker-compose ps
```

`logs` 명령어 : Docker Compose 로그 조회
```bash
# -f  옵션 추가 시 실시간 로그확인 가능
docker-compose logs -f web
```

`run` 명령어 : Docker Compose 컨테이너 내부에서 명령 실행
```bash
docker-compose run [서비스명] [실행 대상 명령]
# docker-compose run web env
```
