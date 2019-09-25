# Loggable

**Loggable** behavior tracks your record changes and is able to manage versions.

- Automatic storage of log entries in database
- Can be nested with other behaviors
- Objects can be reverted to previous versions
- Annotation, Yaml and Xml mapping support for extensions

### Installation
* To get Loggable working you'll have needed to follow the following prerequisites;
    1. Install Laravel doctrine ORM package `composer require laravel-doctrine/orm`
        1. [Optional] If you intend to use the `fluent` driver for schema then you'll need to install Laravel doctrine fluent `composer require laravel-doctrine/fluent`. Please note that this is __NOT__ required if you're using the annotations method.
    2. Install Laravel doctrine extension package `composer require laravel-doctrine/extensions`
    3. Install Gedmo doctrine extension package `composer require gedmo/doctrine-extensions`
* Add `LaravelDoctrine\Extensions\Loggable\LoggableExtension` to the `extensions` array in `doctrine.php` located within the `config` directory of Laravel.
```
'extensions' => [
    //LaravelDoctrine\ORM\Extensions\TablePrefix\TablePrefixExtension::class,
    //LaravelDoctrine\Extensions\Timestamps\TimestampableExtension::class,
    //LaravelDoctrine\Extensions\SoftDeletes\SoftDeleteableExtension::class,
    //LaravelDoctrine\Extensions\Sluggable\SluggableExtension::class,
    //LaravelDoctrine\Extensions\Sortable\SortableExtension::class,
    //LaravelDoctrine\Extensions\Tree\TreeExtension::class,
    LaravelDoctrine\Extensions\Loggable\LoggableExtension::class,
    //LaravelDoctrine\Extensions\Blameable\BlameableExtension::class,
    //LaravelDoctrine\Extensions\IpTraceable\IpTraceableExtension::class,
    //LaravelDoctrine\Extensions\Translatable\TranslatableExtension::class
],
```

* Add Gedmo loggable listeners into the `events` array in `doctrine.php` located within the `config` directory of Laravel.
```
'listeners' => [
    Gedmo\Loggable\LoggableListener::class,
],
```

* Enable Gedmo mappings in the `gedmo` array in `doctrine.php` located within the `config` directory of Laravel.
```
'gedmo' => [
    'all_mappings' => true,
]
```

### Class annotation

> @Gedmo\Mapping\Annotation\Loggable()

This class annotation will store logs to optionally specified logEntryClass.

| Annotations | Description |
|--|--|
| **logEntryClass** |optional entity which stores logs|

### Property annotation

>@Gedmo\Mapping\Annotation\Versioned

This property annotation tracks annotated property for changes

``` php
<?php
namespace Entity;

use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @Gedmo\Loggable
 */
class Article
{
    /** 
    * @ORM\Id
    * @ORM\GeneratedValue 
    * @ORM\Column(type="integer")
    */
    protected $id;

   /**
    * @Gedmo\Versioned
    * @ORM\Column(name="title", type="string", length=8)
    */
   protected $title;
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/loggable.md)
