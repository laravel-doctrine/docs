# Installation in Laravel 5+

Install this package with composer:

```
composer require "laravel-doctrine/extensions:1.0.*"
```

This package wraps extensions from [Gedmo](https://github.com/Atlantic18/DoctrineExtensions) and [Beberlei](https://github.com/beberlei/DoctrineExtensions).

To include Gedmo extensions install them:

```

composer require "gedmo/doctrine-extensions=^2.4"
```

If you are using an **annotation driver**, then add the Gedmo (Behavioral) extensions service provider in `config/app.php`:

```php
LaravelDoctrine\Extensions\GedmoExtensionsServiceProvider::class,
```

To include Beberlei (Query/Type) extensions install them:

```

composer require "beberlei/DoctrineExtensions=^1.0"
```

And then add the Beberlei extensions service provider in `config/app.php`:


```php
LaravelDoctrine\Extensions\BeberleiExtensionsServiceProvider::class,
```
