# Installation in Laravel 6+

The Fluent driver can be easily integrated to a Laravel project that already uses Doctrine
through the [laravel-doctrine/orm](https://github.com/laravel-doctrine/orm) package.

```
composer require laravel-doctrine/fluent
```

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
