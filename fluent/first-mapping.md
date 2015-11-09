# Your first fluent mapping file

Mapping files in the Fluent driver are PHP classes. Each mapping file will be associated with either an `Entity`,
an `Embeddable` or a `MappedSuperClass`. As each of these mappings behave differently for Doctrine, we have a
specific `Mapping` abstract class to extend from:

- `LaravelDoctrine\Fluent\EntityMapping` for Entities
- `LaravelDoctrine\Fluent\EmbeddableMapping` for Embeddables
- `LaravelDoctrine\Fluent\MappedSuperClassMapping` for MappedSuperClasses

## Mapping an entity

Let's say we have a `Scientist` entity that we want to persist to a database.

```php
<?php
namespace App\Entities;

class Scientist
{
    /**
     * @var int
     */
    private $id;

    /**
     * @var string
     */
    private $firstName;

    /**
     * @var string
     */
    private $lastName;

    /**
     * @var \Doctrine\Common\Collections\Collection
     */
    private $theories;

    // methods omitted for simplicity
}
```

Lets start by creating the mapping class for the `Scientist`. For that we will extend
`LaravelDoctrine\Fluent\EntityMapping`.

```php
<?php
namespace App\Mappings;

use App\Entities\Scientist;
use App\Entities\Theory;
use LaravelDoctrine\Fluent\EntityMapping;

class ScientistMapping extends EntityMapping
{
    /**
     * Returns the fully qualified name of the class that this mapper maps.
     *
     * @return string
     */
    public function mapFor()
    {
        return Scientist::class;
    }

    /**
     * Load the object's metadata through the Metadata Builder object.
     *
     * @param Fluent $builder
     */
    public function map(Fluent $builder)
    {
        // This will result in an autoincremented integer
        $builder->increments('id');

        // Both strings will be varchars
        $builder->string('firstName')->nullable();
        $builder->string('lastName')->nullable();

        // This will map an association with Theory as one-to-many
        $builder->hasMany(Theory::class, 'theories');
    }
}
```

As you can see, the syntax used in the `Fluent` builder is similar to what you'd use in
Laravel migrations. The `Builder` object is fully documented in code and with all concrete
methods that will hint on every modern IDE.
