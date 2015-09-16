# Loggable

**Loggable** behavior tracks your record changes and is able to manage versions.

- Automatic storage of log entries in database
- Can be nested with other behaviors
- Objects can be reverted to previous versions
- Annotation, Yaml and Xml mapping support for extensions

### Installation

Add `LaravelDoctrine\Extensions\Loggable\LoggableExtension` to `doctrine.extensions` config.

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
