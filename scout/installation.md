# Installation

First, install the Scout via the Composer package manager:

```
composer require laravel/scout
composer require laravel-doctrine/scout
```

Next, you should add the `ScoutServiceProvider` to the `providers` array of your `config/app.php` configuration file:

```php
    Laravel\Scout\ScoutServiceProvider::class,
    LaravelDoctrine\Scout\ScoutServiceProvider::class,
 ```

After registering the Scout service providers, you should publish the Scout configuration using the `vendor:publish` Artisan command. This command will publish the `scout.php` configuration file to your `config` directory:

```
php artisan vendor:publish
```

The entities that should be searchable should implement `LaravelDoctrine\Scout\Searchable`. 
You can use the `LaravelDoctrine\Scout\Indexable` trait to provide sensible defaults to adhere to the interface.

```php
<?php

class Post implements Searchable
{
    use Indexable;
}
```

The repositories for these entities should extend `LaravelDoctrine\Scout\SearchableRepository`. 

```php
class DoctrinePostRepository extends LaravelDoctrine\Scout\SearchableRepository implements PostRepository
{
}
```

You should bind them to the container as followed:

```php
$this->app->bind(PostRepository::class, function($app) {
    return new DoctrinePostRepository(
        $app['em'],
        $app['em']->getClassMetadata(Post::class),
        $app->make(Laravel\Scout\EngineManager::class)
    );
});
```

Enable the `LaravelDoctrine\Scout\SearchableExtension` in the extensions setting in `doctrine.php` config.

```php
'extensions'                => [
    LaravelDoctrine\Scout\SearchableExtension::class,
];
