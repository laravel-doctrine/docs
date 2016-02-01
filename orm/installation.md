# Installation in Laravel 5+

 Laravel  | Laravel Doctrine
:---------|:----------
 5.1.*    | 1.0.*
 5.2.*    | 1.1.*

Install this package with composer:

```
composer require "laravel-doctrine/orm:1.1.*"
```

After updating composer, add the ServiceProvider to the providers array in `config/app.php`

```php
LaravelDoctrine\ORM\DoctrineServiceProvider::class,
```

Optionally you can register the EntityManager, Registry and/or Doctrine facade:

```php
'EntityManager' => LaravelDoctrine\ORM\Facades\EntityManager::class,
'Registry'      => LaravelDoctrine\ORM\Facades\Registry::class,
'Doctrine'      => LaravelDoctrine\ORM\Facades\Doctrine::class,
```

To publish the config use:

```php
php artisan vendor:publish --tag="config"
```

Available environment variables inside the config are: `APP_DEBUG`, `DOCTRINE_METADATA`, `DB_CONNECTION`, `DOCTRINE_PROXY_AUTOGENERATE`, `DOCTRINE_LOGGER` and `DOCTRINE_CACHE`
