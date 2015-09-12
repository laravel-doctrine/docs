# Installation in Laravel 5+

Install this package with composer:

```
composer require laravel-doctrine/extensions
```

After updating composer, add the ServiceProvider(s) to the providers array in `config/app.php`

To include Gedmo extensions add:

```php
'LaravelDoctrine\Extensions\GedmoExtensionsServiceProvider.php',
```

To include Beberlei extensions add:

```php
'LaravelDoctrine\Extensions\BeberleiExtensionsServiceProvider.php',
```
