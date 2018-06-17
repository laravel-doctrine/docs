# Sortable

**Sortable** behavior will maintain a position field for ordering entities.

- Automatic handling of position index
- Group entity ordering by one or more fields
- Can be nested with other behaviors
- Annotation, Yaml and Xml mapping support for extensions

### Installation

Add `LaravelDoctrine\Extensions\Sortable\SortableExtension` to `doctrine.extensions` config.

### Property annotation

> @Gedmo\Mapping\Annotation\SortableGroup 

This annotation will be used for grouping

> @Gedmo\Mapping\Annotation\SortablePosition

This annotation will be used to store position index

``` php
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
    * @Gedmo\SortablePosition
    * @ORM\Column(name="position", type="integer")
    */
    protected $position;
    
    /**
    * @Gedmo\SortableGroup
    * @ORM\Column(name="category", type="string", length=128)
    */
    protected $category;
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/sortable.md)
