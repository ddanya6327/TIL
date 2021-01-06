# Create a docker image

`docker create <이미지이름>`

## docker image 작성 순서

1. Dockerfile 작성
2. build (docker client)

## dockerfile 만들기

1. 베이스 이미지를 명시
2. 추가적으로 필요한 파일을 다운 받기 위한 몇가지 명령어를 명시
3. 컨테이너 시작시 실행 될 명령어를 명시

Dockerfile 예제

```dockerfile

# Base Image
# ubunt, centOS ...
FROM baseImage

# 필요한 파일을 docker container 안에 copy한다
COPY package.json ./

# 추가적으로 필요한 파일
RUN command

# 컨테이너 시작시 실행될 명령어
CMD ["executable"]
```

- FROM: 이미지 생성시 기반이 되는 이미지 레이어.
  - <이미지 이름>:<태그> 형식으로 작성
  - 태그를 안붙이면 자동적으로 가장 최신것으로 다운 받음
- RUN: 도커 이미지가 생성되기 전에 수행할 쉘 명령어
- CMD: 컨테이너가 시작되었을 때 실행할 실행 파일 또는 셸 스크립트
  - Dockerfile 내 1회만 사용 가능

### base image

- 도커 이미지는 여러개의 레이어로 되어 있다. 그중에서 베이스 이미지는 이 이미지의 기반이 되는 부분. (간단하게 OS라고 생각하면 됨.)

## build

- 해당 디렉토리에서 dockerfile이라는 파일을 찾아서 도커 클라이언트에 전달해준다.

`docker build ./`

### build시 이미지에 대한 이름을 주고 싶다면?

`docker build -t [이미지이름]`

보통 -t [docker id] / [저장소/프로젝트 이름] : [버전] 같은 형태로 이름을 준다.

`docker build -t yang/hello:latest ./`

## copy

필요한 파일이 있다면 copy를 이용해서 container에 파일을 복사 할 수 있다.

```dockerfile
COPY package.json ./

# dockerfile 디렉토리 내의 모든 파일을 container로 복사
COPY ./ ./
```

단, `./ ./` 같은 형태로 COPY 하더라도 아래와 같은 형태로 package.json 은 따로 나눠주는게 좋다.

```dockerfile
COPY package.json ./

RUN npm install

COPY ./ ./
```

그 이유로는 npm install 의 경우 package.json 의 내용에 갱신이 없으면 캐쉬 된 내용을 사용하므로 빠른 속도로 build가 가능하지만, package.json 을 따로 copy 하지 않고 `./ ./` 같은 형태로 복사하면, ./ 디렉토리의 다른 파일의 변경이 있어도 파일 변경으로 인식하므로 package.json의 변화는 없지만 npm install 할 때 다시 종속성을 체크하므로 시간이 오래걸림.

## Working Directory

이미지 안에서 어플리케이션 소스 코드를 갖고 있을 디렉토리를 생성.
이 디렉토리가 어플리케이션의 working directory 가 된다.

별도로 지정해주지 않으면 root directory에 모든 파일을 저장하게 된다. (COPY 파일도)

아래와 같은 느낌

```
bin     dev     liv     mnt    package.json     server.js
...
```

그리고 이름이 같은 파일이 있으면 덮어쓰기를 하기 때문에 문제가 발생할 수 있다.
별도로 파일을 관리하기 위해 working directory를 사용한다.

```dockerfile
FROM node:10

# FROM 다음에 저장할 dir을 입력한다.
WORKDIR /usr/src/app
```

## Volume

데이터를 복사하는 개념과 달리 참조를 함.
예를 들어, server.js 를 copy 한다고 하면 로컬환경 -> 도커 컨테이너로 server.js 파일을 복사하지만, 볼륨의 경우 로컬환경의 server.js를 참조(Mapping) 하므로 참조하는 파일이나 폴더가 수정 되더라도 재빌드를 하지 않아도 된다는 장점이 있다.

아래와 같은 느낌으로 volume을 지정할 수 있다.
`docker run -d -p 5000:8080 -v /usr/src/app/node_modules -v ${pwd}:/usr/src/app yang/nodejs`

volume 의 소스코드가 수정되면 재 빌드 하지 않고 run 만 다시 해주면 소스코드가 갱신된다.

# 주의 할 점

이미지를 생성하더라도 접근이 안 될 경우가 있다.
ex) docker로 express 서버를 구동하는 이미지를 run 해도 localhost:8080에 접근하더라도 재대로 안 뜰 경우가 있음.

이런 경우 내부에 있는 네트워크를 연결 해줘야 한다. (맵핑)

docker 내부의 네트워크가 8080 포트를 사용하고 있다면?
`docker run -p 49160:8080 [이미지이름]`
내 로컬호스트 네트워크 49160 포트를 적용하면 컨테이너의 8080에 맵핑 하겠다는 뜻
