# Installation in Lumen 6+

To set up Laravel Doctrine Extensions in Lumen, we need some additional steps.

Install this package with composer:

```
composer require laravel-doctrine/extensions
```

After updating composer, open `bootstrap/app.php` and register the Service Provider after `LaravelDoctrine\ORM\DoctrineServiceProvider::class`


To include Gedmo extensions install them:

```

composer require "gedmo/doctrine-extensions=^3.0"
```

If you are using an **annotation driver**, then add the Gedmo (Behavioral) extensions service provider in `config/app.php`:

```php
$app->register(LaravelDoctrine\Extensions\GedmoExtensionsServiceProvider::class),
```

To include Beberlei (Query/Type) extensions install them:

```

composer require "beberlei/DoctrineExtensions=^1.0"
```

And then add the Beberlei extensions service provider in `bootstrap/app.php`:


```php
$app->register(LaravelDoctrine\Extensions\BeberleiExtensionsServiceProvider::class),
