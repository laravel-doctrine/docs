# Timestamps

Timestamps allows you to automatically record the time of certain events against your entities. This can be used
to provide similar behaviour to the timestamps feature in Laravel's Eloquent ORM.

Features:

* Automatic predefined date field update on creation, update, property subset update, and even on record property changes
* Specific annotations for properties, and no interface required
* Can react to specific property or relation changes to specific value
* Can be nested with other behaviors
* Annotation, Yaml and Xml mapping support for extensions

### Property annotation

> @Gedmo\Mapping\Annotation\Timestampable 

This annotation tells that this column is timestampable by default it updates this column on update. If column is not date, datetime or time type it will trigger an exception.

| Annotations | Description |
|--|--|
| **on** | is main option and can be create, update, change this tells when it should be updated |
| **field** | only valid if on="change" is specified, tracks property or a list of properties for changes |
| **value** | only valid if on="change" is specified and the tracked field is a single field (not an array), if the tracked field has this value |

```php
<?php
namespace Entity;

use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
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
     * @ORM\Column(type="string", length=128)
     */
    protected $title;

    /**
     * @ORM\Column(name="body", type="string")
     */
    protected $body;

    /**
     * @Gedmo\Timestampable(on="create")
     * @ORM\Column(type="datetime")
     */
    protected $created;

    /**
     * @var \DateTime $updated
     *
     * @Gedmo\Timestampable(on="update")
     * @ORM\Column(type="datetime")
     */
    protected $updated;

    /**

     * @ORM\Column(name="content_changed", type="datetime", nullable=true)
     * @Gedmo\Timestampable(on="change", field={"title", "body"})
     */
    protected $contentChanged;
```

### Timestamps trait

This package provides a trait `LaravelDoctrine\Extensions\Timestamps\Timestamps` to automatically store the `created_at` and `updated_at` time of your entities.

Add the following to the entity you wish to track `created_at` and `updated_at` on

```php

use LaravelDoctrine\Extensions\Timestamps\Timestamps;

class Article {

    use Timestamps;
    
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/timestampable.md)
