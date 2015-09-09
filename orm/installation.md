# Installation

Install this package with composer:

```
composer require laravel-doctrine/orm
```

After updating composer, add the ServiceProvider to the providers array in `config/app.php`

```php
'LaravelDoctrine\ORM\DoctrineServiceProvider',
```

Optionally you can register the EntityManager, Registry and/or Doctrine facade:

```php
'EntityManager' => 'LaravelDoctrine\ORM\Facades\EntityManager',
'Registry'      => 'LaravelDoctrine\ORM\Facades\Registry'
'Doctrine'      => 'LaravelDoctrine\ORM\Facades\Doctrine'
```

To publish the config use:

```php
php artisan vendor:publish --tag="config"
```
