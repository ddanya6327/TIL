# Laravel

PHP에서 사용 할 수 있는 프레임 워크

- MVC 패턴 아키텍처
- ORM (Eloquent) 제공
- PHP 템플릿 Blade 지원

## Composer

PHP 의존성 관리 도구

- Node.js의 NPM, Ruby의 gem 같은 도구

## 기본 Directory

app

- 실제 애플리케이션이 구성되는 디렉토리. 모델, 컨트롤러 등이 있다.

bootstrap

- 라라벨 프레임워크가 실행될 때 부팅에 필요한 파일이 들어있음.

config

- 설정 파일이 들어있음.

database

- 마이그레이션, 시딩, 팩토리 파일을 저장

public

- index.php, 이미지, css, 자바스크립트, download 같은 공개 파일이 저장됨.

resources

- 기타 스크립트 파일, view 템플릿, 다국어 지원 파일, css

routes

- 모든 URL 라우팅을 정의하는 파일이 있음.

storage

- cache, log, 기타 시스템 파일

tests

- 유닛 테스트, 통합 테스트를 위한 테스트 파일들

vendor

- composer가 의존 파일을 설치하는 곳. (.gitignore에 포함 되어 있음.)

## root directory의 파일

.editorconfig

- 라라벨 프로젝트 개발에서 사용하는 코딩 스타일 (들여쓰기, 공백 처리 등, laravel 5.5 이상)

composer.json

- 컴포저 설정 파일. (composer.lock 는 자동 생성)

phpunit.xml

- 라라벨 테스팅 설정 파일(PHP Unit)

server.php

- 라라벨 애플리케이션을 미리 확인할 수 있는 간단한 백업 서버 파일
