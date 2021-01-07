# Travis CI

지속적인 통합(Continuous Integration) 도구

Github와 계정이 연동 됨.

## .travis.yml

TravisCI 에 대한 설정을 해주는 파일

작성 예)

```yml
# 관리자 권한 갖기
sudo: required

# 언어(플랫폼) 선택 ex) python
language: generic

# 서비스 환경 구성 ex) docker
services:
  - docker

# 스크립트를 실행할 수 있는 환경 구성
# 스크립트 실행하기 전에 해야 할 것들
before_install:
  - echo "start Creating an image with dockerfile"
  - docker build -t yang/docker-react-app -f Dockerfile.dev .

# 실행할 스크립트 ex) 테스트
script:
  - docker run -e CI=true yang/docker-react-app npm run test -- --coverage

# 실행 성공 후 할 일
after_success:
  - echo "Test Success"
```

### deploy

```yml
deploy:
  # 외부 서비스 표시 (s3, elasticbeanstalk, firebase 등등)
  provider: elasticbeanstalk
  # 현재 사용하고 있는 AWS의 서비스가 위치하고 있는 물리적 장소
  region: "ap-northeast-1"
  # 생성된 어플리케이션 이름
  app: "docker-react-app"
  # elasticbeanstalk 환경의 이름
  env: "DockerReactApp-env"
  # 해당 elasticbeanstalk s3 버켓 이름
  bucket_name: "elasticbeanstalk-ap-northeast-XXXXX"
  # 어플리케이션 이름과 동일
  bucket_path: "docker-react-app"

  on:
    # github의 브랜치. 이 브랜치에 merge 된 경우 배포를 하겠다.
    branch: master
```
