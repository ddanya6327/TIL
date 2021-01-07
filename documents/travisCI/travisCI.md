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
