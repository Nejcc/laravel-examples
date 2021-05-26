# laravel-examples

## User

### Get data from user

Get object

```php
auth()->user()
```

Get User id

```php
auth()->user()->id
```

```php
auth()->id()
```

Get User email

```php
auth()->user()->email
```

Get User Name

```php
auth()->user()->name
```

### Package relation

- Spatie
  permissions [https://spatie.be/docs/laravel-permission/v4/introduction](https://spatie.be/docs/laravel-permission/v4/introduction)

#### Retrive first assigned role of user

```php
auth()->user()->getRoleNames()[0]
```

### Other helper functions

#### Make "facade" singelton of user instance

```php
if (!function_exists('me')) {
    /**
     * @return \Illuminate\Contracts\Auth\Authenticatable|null
     */
    function me()
    {
        return cache()->remember('me_helper', 600, function () {
            return auth()->user();
        });
    }
}
```

### Use case

```php
me()->email
```

```php
me()->name
```

```php
me()->id
```

## Model - Eloquent related helper

## transforms

#### Implode

```php
return User::all()->pluck('id')->implode();
```

#### Plucking data

```php
return User::all()->pluck('id');
```

#### Relation loading before

```php
$user = User::query()
->with(['user_data_relation'])
->get();
```

### Relation loading before with custom

In this case the last parameter need to be an ID that query know where belong to

```php
$user = User::query()
->with(['user_data_relation' => function($query){
    return $query->select('field_1', 'field_2', 'extra_field', 'id');
}])
->get();
```

In this case we additionaly add subquery filter to filter out all inactive results.

```php
$user = User::query()
->with(['user_data_relation' => function($query){
    return $query->select('field_1', 'field_2', 'extra_field', 'id')->where('is_active', 1);
}])
->get();
```

Example of multi subquery relationship data
```php
$user = User::query()
->with(['posts' => function($query){
    return $query->select('title', 'content',  'created_at', 'id')->where('is_active', 1)
    ->with([
        'comments' => function($query){
            return $query->select('comment', 'created_at', 'id')->where('is_active', 1)
        }
    ]);
}])
->get();
```

#### Relation loading after

```php
$user = User::query()->get();
$user->load('user_data_relation');
```