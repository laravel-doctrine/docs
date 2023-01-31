# Installation in Lumen 6+

To set up Laravel Doctrine ACL in Lumen, we need some additional steps.

Install this package with composer:

```
composer require laravel-doctrine/migrations
```

After updating composer, open `bootstrap/app.php` and register the Service Provider after `LaravelDoctrine\ORM\DoctrineServiceProvider::class`

```php
$app->register(LaravelDoctrine\Migrations\MigrationsServiceProvider::class);
```
