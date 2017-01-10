# Installation in Laravel 5+

 Laravel  | Laravel Doctrine
:---------|:----------
 5.1.*    | 1.0.*
 5.2.*    | 1.1.*
 5.3.*    | 1.2.*

Install this package with composer:

```
composer require "laravel-doctrine/orm:1.2.*"
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

> Important:
> By default, Laravel's application skeleton has its `Model` classes in the `app/` folder. With Doctrine, you'll need to
> create a dedicated folder for your `Entities` and point your `config/doctrine.php` `paths` array to it.
> If you don't, Doctrine will scan your whole `app/` folder for files, which will have a huge impact on performance!
> 
> ```
> 'paths' => [
>     base_path('app/Entities'),
> ],
> ```
