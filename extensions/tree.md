# Tree - Nestedset

Tree nested behavior will implement the standard Nested-Set behavior on your Entity. Tree supports different strategies. Currently it supports nested-set, closure-table and materialized-path. Also this behavior can be nested with other extensions to translate or generated slugs of your tree nodes.

- Materialized Path strategy for ORM
- Closure tree strategy, may be faster in some cases where ordering does not matter
- Support for multiple roots in nested-set
- No need for other managers, implementation is through event listener
- Synchronization of left, right values is automatic
- Can support concurrent flush with many objects being persisted and updated
- Can be nested with other extensions
- Annotation, Yaml and Xml mapping support for extensions

### Class annotation

> @Gedmo\Mapping\Annotation\Tree

| Annotations | Description |
|--|--|
| **type** | this class annotation sets the tree strategy by using the type parameter. Currently nested, closure or materializedPath strategies are supported. An additional "activateLocking" parameter is available if you use the "Materialized Path" strategy with MongoDB. It's used to activate the locking mechanism (more on that in the corresponding section). |

### Property annotation

> @Gedmo\Mapping\Annotation\TreeLeft 

This field is used to store the tree left value

> @Gedmo\Mapping\Annotation\TreeRight 

This field is used to store the tree right value

> @Gedmo\Mapping\Annotation\TreeParent

This will identify the column as the relation to parent node

> @Gedmo\Mapping\Annotation\TreeLevel 

This field is used to store the tree level

> @Gedmo\Mapping\Annotation\TreeRoot 

This field is used to store the tree root id value

> @Gedmo\Mapping\Annotation\TreePath 

(Materialized Path only) This  field is used to store the path. It has an optional parameter "separator" to define the separator used in the path.

> @Gedmo\Mapping\Annotation\TreePathSource 

(Materialized Path only) This field is used as the source to construct the "path"

```php
<?php
namespace Entity;

use Gedmo\Mapping\Annotation as Gedmo;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @Gedmo\Tree(type="nested")
 */
class Category
{
    /** 
    * @ORM\Id 
    * @ORM\GeneratedValue
    * @ORM\Column(type="integer")
    */
    protected $id;

    /**
     * @Gedmo\TreeLeft
     * @ORM\Column(name="lft", type="integer")
     */
    protected $lft;

    /**
     * @Gedmo\TreeLevel
     * @ORM\Column(name="lvl", type="integer")
     */
    protected $lvl;

    /**
     * @Gedmo\TreeRight
     * @ORM\Column(name="rgt", type="integer")
     */
    protected $rgt;

    /**
     * @Gedmo\TreeRoot
     * @ORM\Column(name="root", type="integer", nullable=true)
     */
    protected $root;

    /**
     * @Gedmo\TreeParent
     * @ORM\ManyToOne(targetEntity="Category", inversedBy="children")
     * @ORM\JoinColumn(name="parent_id", referencedColumnName="id", onDelete="CASCADE")
     */
    protected $parent;

    /**
     * @ORM\OneToMany(targetEntity="Category", mappedBy="parent")
     * @ORM\OrderBy({"lft" = "ASC"})
     */
    protected $children;
    
}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/tree.md)
