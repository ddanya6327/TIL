# Eloquent(ORM)

Active Recode ORM 으로, 여러 데이터베이스 작업을 하나의 인터페이스로 처리 할 수 있음.

쉽게 예를 들면 아래와 같다.

```php
$yang = new User();
$yang->email = $request->input('email');
$yang->save();

// find User
// 관례에 따라 테이블명을 알아내고 작업을 수행함. 모델이 User면 테이블은 Users
User::find($userId);
```

## ORM 모델 생성

`php artisan make:model User`

- app/Models/User.php 가 생성됨. (Laravel 7 버전 이하는 app/)

migration 파일도 같이 생성하고 싶다면
`php artisan make:model User --migration`

- 테이블 명은 클래스 명의 복수형을 스네이크 표기법으로 표현한다.
  - 엘로퀀트 모델 클래스명이 SecondaryUser 라면 secondary_users 테이블에 연결.
  - 테이블 명을 명시적으로 지정하려면 모델 클래스의 $table 속성에 값을 입력한다.
    - `$table = 'users'`

### 기본 키

- 기본적으로 테이블의 id를 기본키로 쓰지만, 다른 키를 기본적으로 사용하려면 $primaryKey 속성을 변경한다.
  - 키의 incrementing 기능을 끄려면 $incrementing = false 로 설정해준다.

### 타임스탬프

- 엘로퀀트는 모든 테이블이 created_at과 updated_at 컬럼을 가지고 있다고 생각한다.
  - 없다면 $timestamp = false; 로 기능을 비활성화 하자.

# 데이터 조회

```php
// 엘로퀀트에서만 사용 할 수 있는 메소드 all()
User::all();
// User::get() 과 동작이 동일하므로 get() 을 사용하는게 좋다.

// where을 통한 조건 추가
User::where('vip', true)->get();
```

하나의 레코드 조회

- first(), firstOrFail(), find(), findOrFail()

```php
// ContactController
public function show($contactId)
{
    return view('contacts.show')
        ->with('contact', Contact::findOrFail($contactId));
}
```

### chunk()

- 한 번에 많은 양의 레코드를 처리해야 할 때, chunk() 를 이용해서 레코드를 나누어 처리하면 메모리의 효율을 좋게 할 수 있다.

## count(), sum(), avg()

```php
Contact::where('vip', true)->count();
Contact::sum('votes');
User::avg('age');
```

# 데이터 추가

```php
$contact = new Contact;
$contact->name = 'yang';
$contact->email = 'yang@test.com';
$contact->save();

// or
$contact = new Contact([
    'name' => 'yang',
    'email' => 'yang@test.com'
]);
$contact->save();

// Contact::make(), Contact::create() 메소드를 호출해도 된다.
// 단, create() 메소드는 호출 즉시 DB에 저장된다. save() 메소드를 사용하지 않음.
```

# 데이터 수정

- 수정하고자 하는 인스턴스를 가져온 다음 속성을 수정하고 save 한다.
  - updated_at 이 갱신됨.

```php
$contact = Contact::find(1);
$contact->email = 'yang@test.com';
$contact->save();

// or
$contact = Contact::find(1);
$contact->update(['age' => '30']);
```

# 대량 할당

- 엘로퀀트 클래스의 생성, 수정 메서드에서 배열을 인자로 전달 받는 경우, fillable 속성값을 지정해야 한다.
  - fillable 속성값은 변경하면 안 되는 속성값이 실수로 변경되는 일을 막기 위해 존재.
  - 배열 인자에 여러 값이 전달 되더라도 fillable에 정의된 속성값만 변경된다.

```php
$contact->update([
    'id' => 30, // id의 경우 변경 되면 안 된다.
    'name' => 50,
    'email' => 'yang@test.com',
])
```

- create(), update() 메서드의 경우 fillable, guarded 에 정의된 필드를 참고하여 동작함.
  - 단, $contact->id = 1; 과 같이 속성 값을 직접 지정하는 경우, 대량 할당 기능과 무관하게 작동함.

fillable, guarded 속성 정의

```php
class Contact
{
    protected $fillable = ['name', 'email'];

    // or

    protected $guaraded = ['id', 'created_at', 'updated_at', 'owner_id'];
}
```

# 해당 값이 없는 경우 레코드를 생성

- firstOrCreate(): 데이터베이스에 저장 후 인스턴스를 반환
- firstOrNew(): 인스턴스만 반환

```php
$contact = Contact::firstOrCreate(['email' => 'yang@test.com']);
```

# 데이터 삭제

```php
$contact = Contact::find(5);
$contact->delete();

// or
Contact::where('name', '=', 'yang')->delete();
```

ID를 알고 있는 경우

```php
Contact::destroy(1);
Contact::destroy([1,3,5]);
```

## 소프트 삭제

- 실제 데이터를 삭제하지는 않지만 deleted_at 칼럼을 추가해서 데이터를 검색 할 때 추가하지 않도록 하는 것.
  - 단, 실제로 데이터가 삭제되지 않고 남아있기 때문에 장기적으로 보면 데이터베이스의 크기가 커질 수 있음.
  - quicksend 와 같은 도구로 주기적으로 데이터를 정리하는 것을 권장.
- 사용시 별도의 설정이 필요하다. (검색해볼것)
- 설정시, delete(), destroy() 메소드를 호출해도 레코드를 삭제하지 않고 deleted_at 컬럼에 시간을 기록함.

# 스코프

- 로컬 스코프: 특정 모델에 조건을 정의 할 수 있음
- 글로벌 스코프: 모델의 모든 쿼리에 필터링을 적용 할 수 있음. (ex 소프트 삭제)

## 적용된 글로벌 스코프를 특정 쿼리에서 사용하지 않기

- withoutGlobalScope()

```php
Contact::withoutGlobalScope()->get();
$allContacts = Contact::withoutGlobalScope('active')->get();
```

# 접근자

엘로퀀트 모델에 커스텀 속성을 정의하고 이를 통해 데이터를 읽어오는 기능.

- 주로 읽어오는 데이터의 포맷을 변경할 때, 데이터베이스의 여러 값을 합칠 때 사용

```php
// name 값이 없다면 no name을 반환
class Contact extends Model
{
    public function getNameAttribute($value)
    {
        return $value ?: '(No name)';
    }
}

$name = $contact->name;
```

```php
class Contact extends Model
{
    public function getFullNameAtrribute()
    {
        return $this->first_name . ' ' . $this->last_name;
    }
}

$fullName = $contact->full_name;
```

# 변경자

접근자와 비슷하지만 데이터를 변경할 때 작동.

```php
// amount가 0 이상인 경우만 값을 저장
class Order extends Model
{
    public function setAmountAttribute($value)
    {
        $this->attributes['amount'] = $value > 0 ? $value : 0;
    }
}

$order->amount = '15';
```

# 형변환

값을 읽고, 저장할 때 형 변환되는 기능

```php
class Contact
{
    protected $casts = [
        'vip' => 'boolean',
        'birthday' => 'date',
    ]
}
```

# 날짜 변경자

$dates 배열에 컬럼명을 등록하면 타임스탬프 칼럼으로 작동하도록 정의 할 수 있음.

```php
class Contact
{
    protected $dates = [
        'met_at'
    ]
}
```

## 쿼리 결과 형변환

- withCasts() 메소드를 사용

# 엘로퀀트 컬렉션

엘로퀀트 모델을 사용한 쿼리 결과가 다수의 레코드라면, 엘로퀀트 컬렉션으로 반환 됨.

- https://laravel.kr/docs/8.x/collections
