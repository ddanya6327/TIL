# Route

브라우저 접근용 Web Route는 routes/web.php
API용 Route는 routes.api.php 파일에 정의

## Route 정의

```php
Route::get('/', function() {
    return 'Hello, World!';
})
```

- closure(익명 함수)를 이용해 간단하게 정의 할 수 있다.

```php
Route::get('/', function() {
    return view('welcome');
});

Route::get('about', function() {
    return view('about');
});

Route::get('products', function() {
    return view('products');
});
```

### Route 동사

```php
Route::get('/', function() {
    return 'Hello, World!';
})

Route::post('/', function() {
    // POST
})

Route::put('/', function() {
    // PUT
})

Route::delete('/', function() {
    // DELETE
})

Route::any('/', function() {
    // 요청에 관계 없이 처리
})

Route::match(['get', 'post'], '/', function() {
    // GET OR POST
})
```

## 컨트롤러 메소드를 호출

- `[컨트롤러 클래스, '메소드 이름']` 튜플 형태 또는 `컨트롤러@메서드` 문자열 형태로 호출
  - 라라벨 7까지는 문자열, 8부터는 튜플 형태가 관례

```php
use App\Http\Controllers\WelcomeController;

// App\Http\Controllers\WelcomeController 클래스의 index 메소드를 호출
Route::get('/', [welcomeController::class, 'index']);
```

- 라라벨 7 까지는 컨트롤러 앞에 네임스페이스가 자동으로 붙기 때문에 `Route::get('/', 'WelcomeController@index')` 와 같이 사용했지만, 라라벨 8 부터는 namespace 값이 null이기 때문에 `Route::get('/', 'App\Http\Controllers\WelcomeController@index')` 와 같이 사용하거나, `RouteServiceProvider`의 `$namespace` 속성 값을 변경해야 한다.
  - 튜플 문법은 모든 버전에서도 사용 할 수 있기 때문에 튜플 문법을 사용하는게 좋다.

## Route Parameters

```php
// $id 의 변수명은 {id} 와 일치하지 않아도 된다. 순서만 맞추면 됨.
Route::get('users/{id}/friends', function ($id) {
    //
})

// option
Route::get('users/{id?}', function ($id = 'fallbackId') {
    //
})
```

Parameter에 정규 표현식 추가

```php
Route::get('users/{id}', function ($id) {
    //
})->where('id', '[0-9]+');

Route::get('users/{username}', function ($username) {
    //
})->where('username', '[A-Za-z]+');

Route::get('posts/{id}/{slug}', function ($id, $slug) {
    //
})->where(['id' => '[0-9]+', 'slug' => '[A-Za-z]+']);

// 이 경우, users/abc 라는 요청을 보내면 첫번째는 해당하지 않으므로 실행되지 않고, 두 번째에서 실행 됨.
// posts/abc/123 의 경우 매칭되는 라우트가 없으므로 404 에러
```

## 라우트에 이름 지정하기

```php
Route::get('users/{id}', function ($id) {
    //
})->name('users.show');
// 이름은 어떤 형태로 지정해도 되지만, 관례적으로 [리소스의 복수형.액션] 형태로 한다.
```

route() 헬퍼 함수를 이용해 링크를 표시 할 수 있다.

```php
<a href="<?php echo route('users.show', ['id' => 14]); ?>">
// <a href="http://myapp.com/members/14">

// 파라미터가 없을 경우 routes('users.show')
```

파라미터가 2개 이상의 경우

```php
// users/{userId}/comments/{commentId}

route('users.comments.show', [1, 2])
// http://abc.com/users/1/comments/2

route('users.comments.show', ['userId' => 1, 'commentId' => 2])
// http://abc.com/users/1/comments/2

route('users.comments.show', ['commentId' => 2, 'userId' => 1])
// http://abc.com/users/1/comments/2
```

라우트에 정의되지 않는 파라미터의 경우 쿼리 스트링 형태로 추가 됨.

```php
route('users.comments.show', ['userId' => 1, 'commentId' => 2, 'opt' => 'a'])
// http://abc.com/users/1/comments/2?opt=a
```

## 라우트 그룹

```php
Route::group(function () {
    Route::get('hello', function() {
        return 'Hello';
    });
    Route::get('world', function() {
        return 'World';
    });
})
```

## Route middleware

```php
Route::middleware('auth')->group(function() {
    Route::get('dashboard', function() {
        return view('dashboard');
    });
        Route::get('account', function() {
        return view('account');
    });
})
```

- Controller에도 Middleware를 적용 할 수 있다.

```php
class DashboardController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');

        $this->middleware('admin-auth')
            ->only('editUsers');

        // ...
    }
}
```

## 시간당 접속 제한

- RouteServiceProvider의 RateLimiter 퍼사드의 for 메서드를 이용

## 접두사(prefix)

```php
// 접두사를 붙인 그룹의 '/' 는 '/dashboard' 가 된다.
Route::prefix('dashboard')->group(function () {
    Route::get('/', function () {
        // '/dashboard'
    });
    Route::get('users', function () {
        // '/dashboard/users'
    });
})
```

## 라우트가 어디에도 해당하지 않는 경우

```php
// 라라벨 5.6 이후 버전만 가능
Route::fallback(function () {
    //
})
```

## 서브 도메인

```php
Route::domain('api.myapp.com')->group(function () {
    Route::get('/', function () {
        //
    });
})
```

## View

```php
Route::get('/', function() {
    return view('home');
})

// `resources` 디렉토리의 `views/home.blade.php` 또는 `views/home.php` 파일을 찾음
```

변수를 같이 전달하고 싶은 경우

```php
Route::get('tasks', function() {
    return view('tasks.index')
        ->with('tasks', 'abc');
})

// `resources` 디렉토리의 `views/tasks/index.blade.php` 또는 `views/tasks/index.php` 파일을 찾고, 'abc'값을 tasks라는 변수명으로 전달.
```

View 파일만 보여주면 되는 경우, 간단하게 작성 할 수 있다.

```php
// resources/view/welcome.blade.php
Route::view('/', 'welcome');

// 간단하게 변수를 전달하는 방법
Route::view('/', 'welcome', ['User' => 'Michael']);
```

공유변수가 필요한 경우

```php
view()->share('variableName', 'variableValue');
```

## Resource Controller

리소스 컨트롤러 생성

`php artisan make:controller [컨트롤러 이름] --resource`

리소스 컨트롤러 연결

```php
// routes/web.php

use App\Http\Controller\TaskController;

Route:resource('tasks', TaskController::class);
```

### API 리소스 컨트롤러

라라벨 5.6부터 지원

`php artisan make:controller TaskController --api`

```php
// routes/api.php

use App\Http\Controller\TaskController;

Route:apiResource('tasks', TaskController::class);
```

## 라우트 캐싱

등록된 라우트를 캐시파일로 만듬

`php artisan route:cache`

라우트 캐시 삭제

`php artisan route:clear`

- 캐시된 파일이 있으면 라우트를 수정해도 수정 사항이 반영이 안되므로 갱신 해줘야 한다.

## CORS 처리

config\cors.php 파일에서 설정 가능
