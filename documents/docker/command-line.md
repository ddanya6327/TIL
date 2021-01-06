# Docker Command Line

## docker run

컨테이너 생성 + 컨테이너 실행

`docker run [이미지이름]`

- ex) docker run hello-world

이미지 이름 뒤에 명령어가 오는 경우, 이미지를 실행하지 않고 뒤의 명령어를 실행한다.

- `docker run [이미지이름] ls`

  - ex) docker run alpine ls

- 단, docker image 내부 파일에 따라 명령어를 사용 할 수 없는 경우도 있다.
  - `docker run hello-world ls` 의 경우 정상 작동이 되지 않음.

## 현재 실행중인 컨테이너 나열

`docker ps`

```
CONTAINER ID   IMAGE     COMMAND            CREATED          STATUS          PORTS     NAMES
9d7aa3dac26b   alpine    "ping localhost"   38 seconds ago   Up 36 seconds             jolly_ishizaka
```

CONTAINER ID: 컨테이너 고유 ID 의 일부분
IMAGE: 컨테이너 생성시 사용한 image
COMMAND: 컨테이너 시작시 실행 될 명령어. 대부분 이미지에 내장되어 있으므로 별도의 설정 X
CREATED: 컨테이너 생성 시간
STATUS: 현재 상태
PORTS: 포트(설정하지 않으면 안보임)
NAMES: 컨테이너 고유 이름. 컨테이너 생성시 --name 옵션으로 설정 가능. docker rename 명령어로 변경 가능.

### 원하는 항목만 보고 싶다면?

`docker ps --format 'table{{.Names}} \t {{.Image}}'`

## 모든 컨테이너 나열

`docker ps -a`

## 컨테이너 생성

`docker create [이미지이름]`

## 컨테이너 실행

`docker start [컨테이너 아이디/이름]`

- 이 경우, 컨테이너의 이름만 반환 할 때가 있는데, -a 옵션을 붙여주면 아웃풋 값을 화면에 출력한다.

`docker start -a [컨테이너 아이디/이름]`

## 컨테이너 중지

`docker stop [컨테이너 아이디/이름]`

- 작업들을 완료하고 컨테이너를 중지 시킴

`docker kill [컨테이너 아이디/이름]`

- 바로 컨테이너를 중지 시킴

## 컨테이너 삭제

중지된 컨테이너만 삭제 가능

`docker rm [아이디/이름]`

### 모든 컨테이너 삭제 (mac만 되는 듯?)

```
docker rm `docker ps -a -q`
```

### 이미지 삭제

`docker rmi [이미지id]`

### 컨테이너, 이미지, 네트워크 모두 삭제

도커를 쓰지 않을 때, 모두 정리하고 싶을 때
(실행중인 컨테이너에는 영향을 주진 않음)

`docker system prune`

## 실행중인 컨테이너에 명령 전달

`docker exec [컨테이너 아이디] [명령어]`

- ex) docker exec cabc9846b874 ls

결과적으로 `docker run [이미지이름] ls` 와 `docker exec [이미지이름] ls` 는 같은 결과를 내지만, run은 새로운 컨테이너를 만들어서 실행, exec는 실행중인 컨테이너에 명령을 전달한다.

예를 들어, redis 를 사용하는 경우.
`docker run redis` 명령어로 redis 서버를 실행한 뒤, `redis-cli`를 입력해도 재대로 작동하지 않는다. redis 컨테이너 안에서 `redis-cli` 를 실행해야 하기 때문.
이런 경우 exec 명령어를 사용하여 redis 컨테이너에 접속해서 `redis-cli`를 실행한다.

```
docker run redis

docker exec -it [redis 컨테이너 id] redis-cli
```

## 실행중인 컨테이너의 터미널을 사용하기

컨테이너 안의 쉘이나 터미널에 접속하여 사용한다.

`docker exec -it [컨테이너 아이디] [sh/bash]`

접속 종료시에는 Ctrl + D
