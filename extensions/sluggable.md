# Sluggable

Sluggable behavior will build the slug of predefined fields on a given field which should store the slug

- Automatic predefined field transformation into slug
- Slugs can be unique and styled, even with prefixes and/or suffixes
- Can be nested with other behaviors
- Annotation, Yaml and Xml mapping support for extensions
- Multiple slugs, different slugs can link to same fields

### Installation

Add `LaravelDoctrine\Extensions\Sluggable\SluggableExtension` to `doctrine.extensions` config.

### Property annotation

> @Gedmo\Mapping\Annotation\Slug

This annotation will use the column to store slug generated fields option must be specified, an array of field names to slug

| Annotations | Description |
|--|--|
| **fields** | array of fields that should be slugged |

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
     * @ORM\Column(length=64)
     */
    protected $title;

    /**
     * @ORM\Column(length=16)
     */
    protected $code;

    /**
     * @Gedmo\Slug(fields={"title", "code"})
     * @ORM\Column(length=128, unique=true)
     */
    protected $slug;
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/sluggable.md)
