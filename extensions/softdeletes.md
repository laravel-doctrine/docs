# SoftDeletes

SoftDeleteable behavior allows to "soft delete" objects, filtering them at SELECT time by marking them as with a timestamp, but not explicitly removing them from the database.

- Works with DQL DELETE queries (using a Query Hint).
- All SELECT queries will be filtered, not matter from where they are executed (Repositories, DQL SELECT queries, etc).
- Can be nested with other behaviors
- Annotation, Yaml and Xml mapping support for extensions
- Support for 'timeAware' option: When creating an entity set a date of deletion in the future and never worry about cleaning up at expiration time.

### Installation

Add `LaravelDoctrine\Extensions\SoftDeletes\SoftDeleteableExtension` to `doctrine.extensions` config.

### Class annotation

> @Gedmo\Mapping\Annotation\SoftDeleteable

This class annotation tells if a class is SoftDeleteable. It has a mandatory parameter "fieldName", which is the name of the field to be used to hold the known "deletedAt" field. It must be of any of the date types.

| Annotations | Description |
|--|--|
| **fieldName** | The name of the field that will be used to determine if the object is removed or not (NULL means it's not removed. A date value means it was removed). NOTE: The field MUST be nullable. |

``` php
<?php
namespace Entity;

use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @Gedmo\SoftDeleteable(fieldName="deletedAt", timeAware=false)
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
    * @ORM\Column(name="deletedAt", type="datetime", nullable=true)
    */
    protected $deletedAt;
}
```

A `LaravelDoctrine\Extensions\SoftDeletes\SoftDeletes` trait was included

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/softdeleteable.md)
