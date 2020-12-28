# Query Builder

DB 퍼사드를 이용한다.

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

## select()

- stdClass 객체로 이루어진 배열을 반환

### 파라미터 바인딩

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

## insert()

```php
DB::insert(
    'insert into contacts (name, email) values (?, ?)',
    ['sally', 'sally@me.com']
);
```

## update()

```php
$countUpdated = DB::update(
    'update contacts set status = ? where id = ?',
    ['donor', $id]
);
```

## delete()

```php
$countDeleted = DB::delete(
    'delete from contacts where archived = ?',
    [true]
);
```

# 쿼리 빌더 체이닝

메서드를 체이닝 형태로 호출하여 쿼리를 생성ㅎ라 수 있다.
마지막에 실행 메서드(ex. get())를 호출하면 생성한 쿼리를 실제로 데이터베이스에 실행함.

```php
$usersOfType = DB::table('users')
    ->where('type', $type)
    ->get();
```

## 제약 메서드

- select()

```php
DB:table('users')
    ->select('email', 'email2 as eamil3')
    ->get()

DB:table('users')
    ->select('email')
    ->addSelect('email2 as eamil3')
    ->get()
```

- where()
  - 컬럼, 비교 연산자, 범위 값 3가지의 파라미터를 인자르 받을 수 있음.

```php
$newContacts = DB::table('contact')
    ->where('created_at', '>', now()->subDay())
    ->get();

// '=' 연산자는 생략 가능
// where('vip', true)->get();
```

- orWhere()
  - OR WHERE 구문을 생성

```php
$priorityContacts = DB::table('contacts')
    ->where('vip', true)
    ->orWhere('created_at', '>', now()->subDay())
    ->get();
```

    - 더 복잡한 구문을 만들려면 클로저를 전달한다.
    - where()과 orWhere() 을 같이 사용하면 쿼리가 원하는 대로 실행되지 않을 수 있으므로 주의.

- whereIn(colName, [1,2,3])
  - 주어진 값 중 하나인 경우 레코드를 반환하도록 함.
- whereNotIn(colName, [1,2,3])

- whereNull(colName) , whereNotNull(colName)

  - 주어진 컬럼이 NULL 이거나 NOT NULL인 레코드를 반환

- distinct()
  - 선택한 데이터가 고유할 때만 반환. select() 메서드와 함꼐 사용되는 경우가 많음.

## 쿼리 결과 변경 메서드

- orderBy(colName, direction)

  - asc(오름차순), desc(내림차순)

- groupBy(), having(), havingRaw()
  - 쿼리 결과를 그룹화, having() 으로 결과를 필터링 할 수 있다.

```php
DB::table('contacts')
    ->groupBy('city')
    ->havingRaw('count(contact_id) > 30')
    ->get();
```

- skip(), take()
  - 주로 페이징에 사용.

```php
// 31~40번째 레코드 반환
$page4 = DB::table('contacts')->skip(30)->take(10)->get();
```

- latest(colName), oldest(colName)

  - 해당 칼럼을 기준으로 정렬, 기본적으로 created_at 을 사용.

- inRandomOrder()
  - 결과를 랜덤으로 정렬

## 조건에 따라 쿼리를 추가하는 메서드

- when()
  - 첫 번째 파라미터가 true면 두번째 파라미터의 클로저를 실행함
    - 첫 번째 파라미터가 false일 경우 동작이 필요하다면 3번째 파라미터의 클로저를 넣으면 된다.

```php
$posts = DB::table('posts')
    ->when($ignoreDrafts, function ($query) {
        return $query->where('draft', false);
    })
```

- unless()
  - when과 반대 개념. 조건이 false면 2번째 클로저를 실행

## 쿼리 반환 메소드

- get()

  - 생성된 쿼리를 실행하고 결과를 반환

- first(), firstOrFail()

  - 첫 번째 결과 값을 반환.
  - first()는 결과가 없을 경우 False를 반환.
  - firstOrFail() 은 예외를 발생

- 특정 컬럼에서 값을 조회하려면 컬럼명으로 된 배열 값을 전달하면 된다.

- find(), findOrFail()

  - id를 인자로 받아 검색함. 없을 경우 false를 반환, findOrFail() 의 경우 예외 발생.

- value()
  - 지정된 레코드의 첫 번째 컬럼 값을 반환. first()와 비슷하지만 하나의 컬럼 값을 확인 할 때 사용.

```php
DB::table('contacts')
    ->value('email');
```

- count()

  - 일치하는 레코드의 수를 반환

- min(), max()
  - 주어진 컬럼의 최소 값, 최대 값을 반환

```php
DB::table('orders')->max('amount');
```

- sum(), avg()
  - 주어진 컬럼의 값의 합과 평균을 반환

```php
DB::table('orders')->avg('amount');
```

## 생성되는 SQL 쿼리나 바인딩 되는 변수의 값을 출력

- dd(), dump()

```php
DB::table('users')->where('name', 'Wilbur')->dd();

// "select * from "users" where "name" = ?
// array:1 [ 0 => "Wilbur"]
```

## join

- inner join 을 생성.

```php
DB::table('users')
    ->join('contracts', 'users.id', '=', 'contacts.user_id')
    ->select('users.*', 'contacts.name', 'contacts.status')
    ->get();
```

- 복잡한 join의 경우 클로저를 전달하는 방식으로도 구현 할 수 있다.

## union

```php
$first = DB::table('contacts')
    ->whereNull('first_name');

$contacts = DB::table('contacts')
    ->whereNull('last_name')
    ->union($first)
    ->get();
```

## insert

- 추가하려는 값을 배열로 전달하여 레코드를 추가.

```php
DB::table('users')->insert([
    'name' => 'test',
    'email' => 'test@test.com',
]);

// insert 한 데이터의 id를 얻고 싶다면
$id = DB::table('users')->insertGetId([
    'name' => 'test',
    'email' => 'test@test.com',
]);
```

## update

```php
DB::table('contacts')
    ->where('points', '>', 100)
    ->update(['status' => 'vip']);
```

간단하게 값을 증감, 감소 시키고 싶다면

```php
DB::table('contacts')->increment('tokens', 5); // 5 증가
DB::table('contacts')->decrement('tokens'); // 1 감소
```

## delete

```php
DB::table('users')
    ->where('last_login', '<', now()->subYear())
    ->delete();
```

## JSON 연산

- https://laravel.com/docs/8.x/queries#json-where-clauses

## 트랜잭션

- https://laravel.com/docs/8.x/database#database-transactions
