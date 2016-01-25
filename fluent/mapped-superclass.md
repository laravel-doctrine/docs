# Mapped superclasses

When modeling our entities, we may sometimes find the need to create a hierarchy or family of entities, and do so by
using inheritance in our PHP classes. If we do so, Doctrine offers us several ways to map those classes to our database.
Mapped superclass is one of them.

## What is a mapped superclass

According to [the Doctrine docs](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/inheritance-mapping.html#mapped-superclasses), 
mapped superclasses allow us to share common state between our entities, without it being an entity itself. This means
the parent class will not behave as an entity, and each of our entities will have its own table, containing its state
and, by extension, its parent's state.

> DISCLAIMER: No animals were hurt while modeling these examples!

```php
class TestSubject
{
    /** @var int */
    protected $id;
    
    /** @var Laboratory */
    protected $laboratory;
    
    /** @var HealthStatus */
    protected $healthStatus;
}

class LabRat extends TestSubject
{
    /** @var string */
    private $codeName;
}

class Human extends TestSubject
{
    /** @var string */
    private $firstName;
    
    /** @var string */
    private $lastName;
}
```

While both `TestSubjects` share common state, we don't want to mix them up. So, we'll map them as separate entities and
map the common state as a **Mapped Superclass**:

```php
class TestSubjectMapping extends MappedSuperClassMapping
{
    public function mapFor()
    {
        return TestSubject::class;
    }
    
    public function map(Fluent $builder)
    {
        $builder->belongsTo(Laboratory::class);
        $builder->embed(HealthStatus::class);
    }
}
```

Then, each subclass will have an independent mapping and an independent table:

```php
class LabRatMapping extends EntityMapping
{
    public function mapFor()
    {
        return LabRat::class;
    }
    
    public function map(Fluent $builder)
    {
        $builder->bigIncrements('id');
        
        $builder->string('codeName')->unique();
    }
}

class HumanMapping extends EntityMapping
{
    public function mapFor()
    {
        return Human::class;
    }
    
    public function map(Fluent $builder)
    {
        $builder->bigIncrements('id');
        $builder->string('firstName');
        $builder->string('lastName');
    }
}
```

The previous mapping configuration would result in the following database schema:

| Entity   | Table      | Columns                                                             |
|----------|------------|---------------------------------------------------------------------|
| `LabRat` | `lab_rats` | `id`, `laboratory_id`, `health_status_*`, `code_name UNIQUE`        |
| `Human`  | `humans`   | `id`, `laboratory_id`, `health_status_*`, `first_name`, `last_name` |

Check the [complete mapping reference](/docs/{{version}}/fluent/reference) for more information on inheritance mapping 
and other reuse strategies.