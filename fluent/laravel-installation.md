# Installation in Laravel 5+

The Fluent driver can be easily integrated to a Laravel project that already uses Doctrine
through the [laravel-doctrine/orm](https://github.com/laravel-doctrine/orm) package.


To start using Fluent, edit your `config/doctrine.php` file and choose the `fluent` MetaData
driver:

```php
return [
    // ...
    'managers' => [
        'default' => [
            'meta' => 'fluent',
            'mappings' => [
                // Add your mappings here
                App\Mappings\UserMapping::class,
                App\Mappings\RoleMapping::class,
                // etc...
            ],
    // ...
];
```
