# SQL Query

DB 퍼사드를 이용.

```php
// 쿼리를 직접 전달
DB::statement('drop table users');

// select 메소드
DB::select('select * from users');

// 체이닝을 활용한 방식
$users = DB::table('users')->get();

// 다른 테이블과 JOIN 하는 경우
DB::table('users')
    ->join('contacts', function ($join) {
        $join->on('users.id', '=', 'contacts.user_id')
            ->where('contacts.type', 'donor');
    })
    ->get();
```

select()

- stdClass 객체로 이루어진 배열을 반환

## 파라미터 바인딩

라라벨은 PDO 인터페이스를 사용하므로, 파라미터 바인딩 기능을 사용할 수 있다.

- SQL을 문자열로 조작하는 것보다 바인딩이 SQL 인젝션으로 부터 안전한 코드를 작성하기 쉽다.

바인딩은 ?로 표시하고 인자로 값을 전달하거나

```php
$usersOfType = DB::select(
    'select * from users where type = ?',
    [$type]
);
```

이름을 지정해서 사용 할 수 있다.

```php
$usersOfType = DB::select(
    'select * from users where type = :type',
    ['type' => $userType]
);
```
