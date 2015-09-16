# Translatable

Translatable behavior offers a very handy solution for translating specific record fields in different languages. Further more, it loads the translations automatically for a locale currently used, which can be set to Translatable Listener on it`s initialization or later for other cases through the Entity itself

Features:

- Automatic storage of translations in database
- Automatic translation of Entity or Document fields then loaded
- ORM query can use **hint** to translate all records without issuing additional queries
- Can be nested with other behaviors
- Annotation, Yaml and Xml mapping support for extensions

### Class annotation

> @Gedmo\Mapping\Annotation\Translatable

| Annotations | Description |
|--|--|
| **class** | it will use this class to store translations generated |

### Property annotation

> @Gedmo\Mapping\Annotation\Translatable

It will translate this field

> @Gedmo\Mapping\Annotation\Locale or @Gedmo\Mapping\Annotation\Language

This will identify this column as locale or language used to override the global locale

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
     * @Gedmo\Translatable
     * @ORM\Column(name="title", type="string", length=128)
     */
    protected $title;

    /**
     * @Gedmo\Translatable
     * @ORM\Column(name="content", type="text")
     */
    protected $content;

    /**
     * @Gedmo\Locale
     * Used locale to override Translation listener`s locale
     * this is not a mapped field of entity metadata, just a simple property
     */
    protected $locale;
    
}
```

### Translation Entity

```
<?php

use Doctrine\ORM\Mapping as ORM;
use Gedmo\Translatable\Entity\MappedSuperclass\AbstractTranslation;

/**
 * @ORM\Table(name="article_translations", indexes={
 * @ORM\Index(name="article_translation_idx", columns={"locale", "object_class", "field", "foreign_key"})
 */
class ArticleTranslation extends AbstractTranslation
{
    /**
     * All required columns are mapped through inherited superclass
     */
}

```

```
<?php
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @Gedmo\TranslationEntity(class="ArticleTranslation")
 */
class Article
{
    
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/translatable.md)
