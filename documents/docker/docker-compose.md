# Docker-compose

예를 들어, 컨테이너 A (nodeJS + Redis Client)
컨테이너 B (Redis Server)

A컨테이너에서 B컨테이너에 접근해야 하는 상황.

멀티 컨테이너 상황에서 아무 설정 해주지 않으면 컨테이너간에 접근 할 수 없다.
쉽게 네트워크를 연결시켜주기 위해서 Docker Compose를 이용하면 쉽다.

- 도커 실행 명령이 긴 경우에도 docker-compose를 사용하면 간단하게 사용할 수 있다.

`docker-compose.yml` 파일에 내용을 작성한다.

## docker-compose.yml

```yml
version: "3" # docker-compose 버전
services: # 실행하려는 컨테이너들
  redis-server: # 컨테이너
    image: "redis" # 컨테이너에 사용하려는 이미지
  node-app: # 컨테이너
    build: . # 현 디렉토리에 있는 dockerfile
    ports: # port mapping
      - "5000:8080"
```

## 실행

docker-compose.yml 이 있는 디렉토리에서

`docker-compose up`

- 이미지가 없을때 이미지를 빌드하고 컨테이너 시작

`docker-compose up --build`

- 이미지가 있든 없든 이미지를 빌드하고 컨테이너 시작, 소스의 수정이 있거나 하면 --build 로 하는 편이 좋다.

백그라운드에서 실행하고 싶다면

`docker-compose up -d --build`

## 중지

`docker-compose down`

# 주의점

## containers간 통신 할 때 나타나는 에러

## 컨테이너 실행 순서

예를 들어, node 환경에서 redis를 사용 할 경우.

올바른 순서

1. redis server 실행
2. node 실행

순서가 반대로 된다면 redis server가 실행되어 있지 않기 때문에 node 실행시 에러가 발생할 수 있다.
