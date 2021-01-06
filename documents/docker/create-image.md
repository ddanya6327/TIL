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

### 베이스 이미지

- 도커 이미지는 여러개의 레이어로 되어 있다. 그중에서 베이스 이미지는 이 이미지의 기반이 되는 부분. (간단하게 OS라고 생각하면 됨.)

## build

- 해당 디렉토리에서 dockerfile이라는 파일을 찾아서 도커 클라이언트에 전달해준다.

`docker bwuild ./`

### build시 이미지에 대한 이름을 주고 싶다면?

`docker build -t [이미지이름]`

보통 -t [docker id] / [저장소/프로젝트 이름] : [버전] 같은 형태로 이름을 준다.

`docker build -t yang/hello:latest ./`
