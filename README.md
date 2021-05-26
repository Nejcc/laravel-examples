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
- Sppatie permissions [https://spatie.be/docs/laravel-permission/v4/introduction](https://spatie.be/docs/laravel-permission/v4/introduction)

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

### Usecase
```php
me()->email
```
```php
me()->name
```
```php
me()->id
```
