# Organisations

A lot of applications have an organisations structure. Teams, Organisations, Offices, ... To add this functionality to your Application, you will have
to create an entity that implements `LaravelDoctrine\ACL\Contracts\Organisation`. Next change `acl.organisations.entity` to your entity.

```php
<?php

namespace App;

use Doctrine\ORM\Mapping as ORM;
use LaravelDoctrine\ACL\Contracts\Organisation;
use LaravelDoctrine\ACL\Mappings as ACL;

/**
 * @ORM\Entity
 */
class Team implements Organisation
{
    /**
     * @ORM\Column(type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     */
    protected $id;

    /**
     * @ORM\Column(type="string")
     */
    protected $name;

    /**
     * @return int
     */
    public function getId()
    {
        return $this->id;
    }

    /**
     * @return string
     */
    public function getName()
    {
        return $this->name;
    }
}
```

### User can belong to one organisation

The User class should implement `LaravelDoctrine\ACL\Contracts\BelongsToOrganisation`. You can use the `@ACL\BelongsToOrganisation` annotation to define the relation.

```php
<?php

use Doctrine\ORM\Mapping as ORM;
use LaravelDoctrine\ACL\Mappings as ACL;
use LaravelDoctrine\ACL\Contracts\BelongsToOrganisation;

/**
 * @ORM\Entity
 */
class User implements BelongsToOrganisation
{
    /**
     * @ACL\BelongsToOrganisation
     * @var Organisation
     */
    protected $organisation;
    
    /**
     * @return Organisation
     */
    public function getOrganisation()
    {
        return $this->organisation;
    }
}
```

### User can belong to multiple organisation

The User class should implement `LaravelDoctrine\ACL\Contracts\BelongsToOrganisations`. You can use the `@ACL\BelongsToOrganisations` annotation to define the relation.


```php
<?php

use Doctrine\ORM\Mapping as ORM;
use LaravelDoctrine\ACL\Mappings as ACL;
use LaravelDoctrine\ACL\Contracts\BelongsToOrganisations;

/**
 * @ORM\Entity
 */
class User implements BelongsToOrganisations
{
    /**
     * @ACL\BelongsToOrganisations
     * @var Organisation[]
     */
    protected $organisations;
    
    /**
     * @return Organisation[]
     */
    public function getOrganisations()
    {
        return $this->organisations;
    }
}
```
