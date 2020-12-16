# Config

라라벨 애플리케이션의 설정 파일은 config 디렉토리에 위치한다.
각 설정 파일은 PHP 배열을 반환하는데 배열의 값은 파일명과 배열 키를 (.) 으로 구분한다.

## config의 값 가져오기

```php
// config/test.php
<?php
return [
    'tools' => [
        'PHPTest' => 'PHPUnit'
    ]
];
```

- 위의 파일에 접근하기 위해서는 `config('test.tools.phptest')` 와 같은 형태로 접근한다.

.env에서 값을 가져오려면?

```php
// config/test.php
<?php
return [
    'tools' => [
        'PHPTest' => env('PHP_TEST_TOOL'),
    ]
];
```

단, 설정파일(config) 밖에서는 env() 를 사용 할 수 없다. 내부 애플리케이션에서 .env를 참조하고 싶다면 위와 같이 config에 env의 값을 불러오고 내부 애플리케이션에서 참조 하도록 하는게 좋다.

## .env

APP_KEY

- 데이터를 암호화 하는데 사용한다. 이 값이 빈 채로 애플리케이션을 실행하면 에러 메시지가 출력 됨. `php artisan key:generate` 명령어로 이 값을 자동으로 생성 가능.

APP_DEBUG

- boolean 값으로 로컬 개발 환경이나 스테이지 환경에서 에러 디버깅 정보를 출력할지 말지 설정. 실제 환경에서는 절대 활성화 하지 않도록.

DB_DATABASE, DB_USERNAME, DB_PASSWORD

- 데이터 베이스의 설정
