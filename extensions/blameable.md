# Blameable

Blameable behavior will automate the update of username or user reference fields on your Entities or Documents. It works through annotations and can update fields on creation, update, property subset update, or even on specific property value change.

* Automatic predefined user field update on creation, update, property subset update, and even on record property changes
* Specific annotations for properties, and no interface required
* Can react to specific property or relation changes to specific value
* Can be nested with other behaviors
* Annotation, Yaml and Xml mapping support for extensions

### Installation

Add `LaravelDoctrine\Extensions\Blameable\BlameableExtension` to `doctrine.extensions` config.

### Property annotation

> @Gedmo\Mapping\Annotation\Blameable

This annotation tells that this column is blameable
by default it updates this column on update. If column is not a string field or an association
it will trigger an exception.

Available configuration options:

| Annotations | Description |
|--|--|
| **on** |is main option and can be **create, update, change** this tells when it should be updated|
| **field** | only valid if **on="change"** is specified, tracks property or a list of properties for changes |
| **value** | only valid if **on="change"** is specified and the tracked field is a single field (not an array), if the tracked field has this **value** then it updates the blame |

Column is a string field:

``` php
<?php

namespace App\Entities;

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
     * @var string $createdBy
     *
     * @Gedmo\Blameable(on="create")
     * @ORM\Column(type="string")
     */
    protected $createdBy;

    /**
     * @var string $updatedBy
     *
     * @Gedmo\Blameable(on="update")
     * @ORM\Column(type="string")
     */
    protected $updatedBy;

    /**
     * @var string $contentChangedBy
     *
     * @ORM\Column(name="content_changed_by", type="string", nullable=true)
     * @Gedmo\Blameable(on="change", field={"title", "body"})
     */
    protected $contentChangedBy;
}
```

Column is an association:

```php
<?php

namespace App\Entities;

use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 */
class Article
{
    /**
     * @Gedmo\Blameable(on="create")
     * @ORM\ManyToOne(targetEntity="App\Entities\User")
     * @ORM\JoinColumn(name="created_by", referencedColumnName="id")
     */
    protected $createdBy;
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/blameable.md)
