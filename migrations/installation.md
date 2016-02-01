# Installation in Laravel 5+

Install this package with composer:

```
composer require "laravel-doctrine/migrations:1.0.*"
```

After updating composer, add the ServiceProvider to the providers array in `config/app.php`

```php
LaravelDoctrine\Migrations\MigrationsServiceProvider::class,
```

To publish the config use:

```php
php artisan vendor:publish --tag="config"
```
