# Ip Traceable

IpTraceable behavior will automate the update of IP trace on your Entities or Documents. It works through annotations and can update fields on creation, update, property subset update, or even on specific property value change.

* Automatic predefined ip field update on creation, update, property subset update, and even on record property changes
* Specific annotations for properties, and no interface required
* Can react to specific property or relation changes to specific value
* Can be nested with other behaviors
* Annotation, Yaml and Xml mapping support for extensions

> @Gedmo\Mapping\Annotation\IpTraceable

This annotation tells that this column is ipTraceable by default it updates this column on update. If column is not a string field it will trigger an exception.

Available configuration options:

| Annotations | Description |
|--|--|
| **on** |is main option and can be **create, update, change** this tells when itshould be updated|
| **field** | only valid if **on="change"** is specified, tracks property or a list of properties for changes |
| **value** | only valid if **on="change"** is specified and the tracked field is a single field (not an array), if the tracked field has this **value**then it updates the trace |

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
     * @ORM\Column(type="string", length=128)
     */
    protected $title;

    /**
     * @ORM\Column(name="body", type="string")
     */
    protected $body;

    /**
     * @var string $createdFromIp
     *
     * @Gedmo\IpTraceable(on="create")
     * @ORM\Column(type="string", length=45, nullable=true)
     */
    protected $createdFromIp;

    /**
     * @var string $updatedFromIp
     *
     * @Gedmo\IpTraceable(on="update")
     * @ORM\Column(type="string", length=45, nullable=true)
     */
    protected $updatedFromIp;

    /**
     * @var datetime $contentChangedFromIp
     *
     * @ORM\Column(name="content_changed_by", type="string", nullable=true, length=45)
     * @Gedmo\IpTraceable(on="change", field={"title", "body"})
     */
    protected $contentChangedFromIp;
}
```

A `LaravelDoctrine\Extensions\IpTraceable\IpTraceable` trait was included which set's the IP on *create* and *update*

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/ip_traceable.md)
