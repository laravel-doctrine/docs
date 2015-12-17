# Mapping Reference

This is the complete Fluent mapping reference.

- [Table](#table)
- [Columns](#columns)
- [Relations](#relations)
  - [*-to-one relations](#relations-to-one)
    - [oneToOne](#relations-one-to-one)
    - [hasOne](#relations-has-one)
    - [manyToOne](#relations-many-to-one)
    - [belongsTo](#relations-belongs-to)
  - [*-to-many relations](#relations-to-many)
    - [oneToMany](#relations-one-to-many)
    - [hasMany](#relations-has-many)
    - [manyToMany](#relations-many-to-many)
    - [belongsToMany](#relations-belongs-to-many)
- [Embeddables](#embeddables)
- [Inheritance mapping](#inheritance)
- [Lifecycle callbacks](#lifecycle-callbacks)
- [Primary keys](#primary-keys)
- [Indexes](#indexes)
- [Unique Constraints](#uniques)
- [Mapping overrides](#overrides)
- [Extending with macros](#macros)
- [Advanced Doctrine Entity configuration](#advanced)

<a name="table"></a>
## Table

Each `Entity` defined in Doctrine will have a table associated with it. The name of the table is automatically
generated for you through a `NamingStrategy`, but you can change it if you need to:

```php
$builder->table('science_guys');
```

<a name="columns"></a>
## Columns
Columns are defined through their type and associated field. In Doctrine, we define fields in our
entities and then create a database schema based on them, with a given `NamingStrategy`.

*IMPORTANT*: All `$name` variables used here expect the *field name*. That means the name of the property inside your
 Entity, *not the column name you expect to have in your database*. Also, *fields have to be explicitly declared in the
 Entity class*.

### Base type fields

| Method                                    | Description |
|-------------------------------------------|-------------|
| `$builder->bigInteger('votes')`           | Maps a `int` field with the given `$name` to a `BIGINT` column. |
| `$builder->binary('data')`                | Maps a `string` field with the given `$name` to a `BINARY` column. |
| `$builder->blob('data')`                  | Maps a `string` field with the given `$name` to a `BLOB` column. Encoding and decoding will be handled by Doctrine. |
| `$builder->boolean('confirmed')`          | Maps a `boolean` field with the given `$name` to a `BOOLEAN` column. |
| `$builder->decimal('amount')`             | Maps a `float` field with the given `$name` to a `DECIMAL` column. |
| `$builder->float('amount')`               | Maps a `float` field with the given `$name` to a `FLOAT` column. |
| `$builder->guid('id')`                    | Maps a `string` field with the given `$name` to a `GUID` or `TEXT` column. Using this in platforms that don't support it natively is not recommended: use `$builder->binary()->lengh(16)` instead. |
| `$builder->integer('votes')`              | Maps a `int` field with the given `$name` to a `INTEGER` column. |
| `$builder->jsonArray('data')`             | Maps an `array` field with the given `$name` to a `JSON` or `TEXT` column. Encoding and decoding will be handled by Doctrine. |
| `$builder->object('data')`                | Maps a `serializable` field with the given `$name` to a `CLOB` or `TEXT` column, containing a serialized string. Encoding and decoding will be handled by Doctrine. |
| `$builder->array('data')`                 | Maps an `array` field with the given `$name` to a `CLOB` or `TEXT` column, containing a serialized string. Encoding and decoding will be handled by Doctrine. |
| `$builder->simpleArray('data')`           | Maps a `string[]` field with the given `$name` to a `CLOB` or `TEXT` column, containing a comma separated list of elements. Encoding and decoding will be handled by Doctrine. |
| `$builder->smallInteger('votes')`         | Maps a `int` field with the given `$name` to a `SMALLINT` column. |
| `$builder->string('firstName')`           | Maps a `string` field with the given `$name` to a `VARCHAR` column. |
| `$builder->text('bio')`                   | Maps a `string` field with the given `$name` to a `TEXT` column. |
| `$builder->field('CUSTOM_TYPE', 'foo')`   | Generic method for adding any type of field that Doctrine supports. You can find type constants in `Doctrine\DBAL\Types\Type`, or use your own. |

### Date fields

| Method                                      | Description |
|---------------------------------------------|-------------|
| `$builder->date('birthday')`                | Maps a `DateTime` field with the given `$name` to a `DATE` column. |
| `$builder->dateTime('registeredAt')`        | Maps a `DateTime` field with the given `$name` to a `DATETIME` column. |
| `$builder->dateTimeTz('appointment')`       | Maps a `DateTime` field with the given `$name` to a `DATETIME WITH TIMEZONE` column. |
| `$builder->time('alarm')`                   | Maps a `DateTime` field with the given `$name` to a `TIME` column. |
| `$builder->carbonDate('birthday')`          | Maps a `Carbon\Carbon` field with the given `$name` to a `DATE` column. |
| `$builder->carbonDateTime('registeredAt')`  | Maps a `Carbon\Carbon` field with the given `$name` to a `DATETIME` column. |
| `$builder->carbonDateTimeTz('appointment')` | Maps a `Carbon\Carbon` field with the given `$name` to a `DATETIME WITH TIMEZONE` column. |
| `$builder->carbonTime('alarm')`             | Maps a `Carbon\Carbon` field with the given `$name` to a `TIME` column. |
| `$builder->zendDate('registeredAt')`        | Maps a `Zend_Date` field with the given `$name` to a `DATETIME` column. |

### Syntax sugar

This driver adds some custom syntax methods to ease frequent use cases.

| Method                                    | Description |
|-------------------------------------------|-------------|
| `$builder->bigIncrements('id')`           | Alias of `$builder->bigInteger('id')->unsigned()->primary()->autoIncrement()` |
| `$builder->increments('id')`              | Alias of `$builder->integer('id')->unsigned()->primary()->autoIncrement()` |
| `$builder->smallIncrements('id')`         | Alias of `$builder->smallInteger('id')->unsigned()->primary()->autoIncrement()` |
| `$builder->unsignedBigInteger('votes')`   | Alias of `$builder->bigInteger('votes')->unsigned()` |
| `$builder->unsignedInteger('votes')`      | Alias of `$builder->integer('votes')->unsigned()` |
| `$builder->unsignedSmallInteger('votes')` | Alias of `$builder->smallInteger('votes')->unsigned()` |
| `$builder->timestamp('registeredAt')`     | Alias of `$builder->carbonDateTime('registeredAt')`. |
| `$builder->timestampTz('appointment')`    | Alias of `$builder->carbonDateTimeTz('appointment')`. |
| `$builder->rememberToken()`               | Alias of `$builder->string('rememberToken')->nullable()->length(100)`, as expected for Laravel's `Auth` driver. Field name and further customization is available as with any `Field` method. |

### Field customizations

Field customizations apply to all fields described above.

All column methods return an instance of `LaravelDoctrine\Fluent\Builders\Field` and, if the user
prefers, an optional callback can be used instead of the _fluent_ interface. This callback will
receive the same `LaravelDoctrine\Fluent\Builders\Field` instance as the only parameter, so both options are identical.

```php
// Using the returned object
$builder->string('description')
    ->length(240)
    ->nullable();

// Using the callback
$builder->string('description', function(Field $field){
    $field->length(240)->nullable();
});
```

Most fields can be customized with the following methods, when it makes sense. Read [the Doctrine Annotations documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/annotations-reference.html#column)
for an analogue implementation.

| `Field` method | Description |
|----------------|-------------|
| `$field->collation('utf16_general_ci')`            | The collation of the column (only supported by Drizzle, Mysql, PostgreSQL>=9.1, Sqlite and SQLServer). |
| `$field->columnDefinition('VARCHAR(33) NOT NULL')` |  DDL SQL snippet that starts after the column name and specifies the complete (non-portable!) column definition. This attribute allows to make use of advanced RMDBS features. However you should make careful use of this feature and the consequences. SchemaTool will not detect changes on the column correctly anymore if you use “columnDefinition”. |
| `$field->columnName('my_custom_name')`             | Set a custom name to the column. If not set, a column name will be generated automatically, based on the field and the current `NamingStrategy`. |
| `$field->comment('This column is awesome!')`       | The comment of the column in the schema (might not be supported by all vendors). |
| `$field->default(false)`                           | Set the default value of the column. Although this may seem useful, default values can be more flexible by setting them on entities. |
| `$field->fixed(true)`                              | Boolean value to determine if the specified length of a string column should be fixed or varying (applies only for string/binary column and might not be supported by all vendors). |
| `$field->length(60)`                               | Used by the `string` type to determine its maximum length in the database. Doctrine does not validate the length of string values for you. |
| `$field->name('my_custom_name')`                   | Alias of `$field->columnName('my_custom_name')`. |
| `$field->nullable()`                               | Allows NULL values for this column. |
| `$field->option($name, $value)`                    | Set custom options. |
| `$field->precision(8)`                             | The precision for a decimal (exact numeric) column (applies only for decimal column), which is the maximum number of digits that are stored for the values. |
| `$field->scale(3)`                                 | The scale for a decimal (exact numeric) column (applies only for decimal column), which represents the number of digits to the right of the decimal point and must not be greater than precision. |
| `$field->unique()`                                 | Adds a unique constraint to the column. |
| `$field->unsigned()`                               | Set the current field (must be numeric) to `unsigned`. |
| `$field->useForVersioning()`                       | Defines the specified column as version attribute used in an optimistic locking scenario. It only works on integer or datetime columns. Combining `useForVersioning()` with `primary()` is not supported. |

### Generated values

Doctrine can be configured to use automatically generated values on the database side.
This is usually used to configure auto-incrementing integers for primary columns.

This is an advanced feature, so [reading the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/basic-mapping.html#identifiers-primary-keys)
is *very recommended*.

You can access the `LaravelDoctrine\Fluent\Builders\GeneratedValue` object only through a `Field` object,
by using a `callable $callback`. For example:

```php
$builder->integer('counter')->generatedValue(function(GeneratedValue $gen){
    // Here you can use the GeneratedValue to customize it.
});
```

If what you need is an auto-incrementing field, we have you covered:

```php
$builder->integer('counter')->autoIncrement();
```

The following methods can be found on the `GeneratedValue` object:

```php
$gen->auto(string $name = null, int $initial = null, int $size = null): GeneratedValue
```

Tells Doctrine to pick the strategy that is preferred by the used database platform. The preferred strategies
are IDENTITY for MySQL, SQLite, MsSQL and SQL Anywhere and SEQUENCE for Oracle and PostgreSQL. This strategy
provides full portability. This is the default strategy.

If parameters are given, they will be used to configure the `sequenceGenerator`.

```php
$gen->sequence(string $name = null, int $initial = null, int $size = null): GeneratedValue
```

Tells Doctrine to use a database sequence for ID generation. This strategy does currently not provide full
portability. Sequences are supported by Oracle, PostgreSql and SQL Anywhere.

If parameters are given, they will be used to configure the `sequenceGenerator`.

```php
$gen->identity(): GeneratedValue
```

Tells Doctrine to use special identity columns in the database that generate a value on insertion of a row.
This strategy does currently not provide full portability and is supported by the following platforms:

MySQL/SQLite/SQL Anywhere (AUTO_INCREMENT), MSSQL (IDENTITY) and PostgreSQL (SERIAL).

```php
$gen->uuid(): GeneratedValue
```

Tells Doctrine to use the built-in Universally Unique Identifier generator.
This strategy provides full portability.

*NOTE*: This UUIDs will be generated on the database. To use application-side UUIDs, you should use either a `guid()`
field (PostgreSQL) or a `binary()->length(16)` field (MySQL), and create / receive the corresponding UUID on your
Entity's `__construct()` method.

```php
$gen->none(): GeneratedValue
```

Tells Doctrine that the identifiers are assigned (and thus generated) by your code. The assignment must take
place before a new entity is passed to EntityManager#persist.

NONE is the same as leaving off the GeneratedValue entirely.

```php
$gen->custom(string $generatorClass): GeneratedValue
```

Tells Doctrine to use a custom Generator class to generate identifiers.
The given class must extend `Doctrine\ORM\Id\AbstractIdGenerator`

<a name="relations"></a>
## Relations

In Doctrine, relations associate entities to one another, and define the necessary database columns and
tables to do so. Relations have directionality: they can be unidirectional or bidirectional.

Unidirectional relations are one-way relations, where an owning entity knows about another, but the other
doesn't know about the former. On the other hand, bidirectional relations are declared on both entities.
When using bidirectional relations, it is important to know which side is considered the _owner_ and which
the _inverse_ side, as this will affect how Doctrine processes it.

Relations are defined associating the current entity with another entity by its fully qualified
class name. All relations will return a specific `LaravelDoctrine\Fluent\Relations\Relation` child object
that can be used to further define it.

The relation field can be deduced by the driver based on the referenced class name, using singular or
plural depending on the relation type, so all `$field` parameters are optional.

This driver also provides alias methods to have Laravel's syntax in Doctrine.

<a name="relations-to-one"></a>
### *-to-one relations

"To one" relations on Doctrine define an association where one Entity (the owner) will have a direct
relationship with another (the inverse.)

By default, the current entity will hold a proxy instance of the related entity until it is accessed, unless
an `eager loading` fetch mode is configured or a join was used to fetch both. In any case, you can use the
property holding the relation as if it were already loaded from the database.

<a name="relations-one-to-one"></a>
#### oneToOne

Define a `one-to-one` relation. If not defined, the current entity will be the owning side of the relation
(that means it will be the one holding the foreign key.)

This is Doctrine's default way of defining this kind of relations.

```php
// Owning side (particularily, this is a self-referencing relation)
$builder->oneToOne(Scientist::class, 'mentor')->inversedBy('apprentice');

// Inverse side (LabCoat class has an 'owner' property)
$builder->oneToOne(LabCoat::class)->mappedBy('owner');
```

<a name="relations-has-one"></a>
#### hasOne

Inspired by Laravel's relation syntax, this method maps a `one-to-one` *inverse side* relation.
This means that the current entity does not hold the foreign key, but is referenced by the given `$entity`.
As the relation is a `one-to-one`, a unique constraint will be added on the owning side to prevent
multiple objects being related to this one.

```php
// Deduces the 'profile' field for you.
$builder->hasOne(Profile::class);

// Uses the 'coat' field.
$builder->hasOne(LabCoat::class, 'coat');
```

*IMPORTANT*: This is *NOT* a direct alias of `oneToOne($entity)`. It is closer to a version of
`oneToMany`, restricted to only one related entity on the inverse side by a `unique` constraint.

<a name="relations-many-to-one"></a>
#### manyToOne

This method maps the owning side of either a `one-to-one` or a `one-to-many` relation. Contrary to the `oneToOne` scenario,
in this case the inverse side is not restricted to only one entity.

```php
$builder->manyToOne(Scientist::class, 'mentor');
$builder->manyToOne(University::class);
```

<a name="relations-belongs-to"></a>
#### belongsTo

Inspired by Laravel's relation syntax, this method maps the owning side of a `one-to-one` relation.
This uses the `ManyToOne` implementation, as it is exactly the same when seen from the owning side.

```php
$builder->belongsTo(Scientist::class, 'mentor');
$builder->belongsTo(University::class);
```

<a name="relations-to-many"></a>
### *-to-many relations

"To many" relations on Doctrine define an association where one Entity (the owner) will have a collection
of the another entity (the inverse.) In the `many-to-many` scenario, both ends of the relation will hold
collections of each other, but still one of them will have to be defined as the owner of the relationship.

Doctrine will use implementations of the `Doctrine\Common\Collections\Collection` interface when it loads
the entities from database. By default, a lazy loading collection will be given to all entities that hold
`*-to-many` relations, so that the related collection is only accessed when needed. Collections will be
populated for you when you set eager loading or when you do join queries.

What *you* need to do with your entities is *ALWAYS* initialize your `*-to-many` relations with a new
 `Doctrine\Common\Collections\ArrayCollection` object.

```php
use Doctrine\Common\Collections\ArrayCollection;

class Scientist
{
    // ...

    /** @var \Doctrine\Common\Collections\Collection */
    private $theories;

    public function __construct()
    {
        // ...

        $this->theories = new ArrayCollection();
    }
}
```

This way you always have a `Collection` object in your property and you can safely use it.

<a name="relations-one-to-many"></a>
#### oneToMany

Defines the inverse side of the `many-to-one` relation described above. If not provided, the camel-cased, pluralized
form of the related entity's name will be used as the field.

```php
// Assumes a 'theories' field will hold the Collection
$builder->oneToMany(Theory::class);

// Uses the 'papers' field
$builder->oneToMany(Publication::class, 'papers');
```

The `one-to-many` relations in Doctrine *cannot be defined as unidirectional from the inverse side*
without using a join table. To do so, you have to define a `many-to-many` relation instead, and set the joined
column as unique:

```php
// you can either...
$builder->manyToMany(Award::class, 'awards')
    ->joinColumn('scientist_id', 'award_id', true);

// or...
$builder->belongsToMany(Award::class, 'awards')
   ->joinColumn('scientist_id', 'award_id', true);
```

<a name="relations-has-many"></a>
#### hasMany

Inspired by Laravel's relation syntax, this is an alias of `$builder->oneToMany($entity)`.

```php
// Assumes a 'theories' field will hold the Collection
$builder->hasMany(Theory::class);

// Uses the 'papers' field
$builder->hasMany(Publication::class, 'papers');
```

<a name="relations-many-to-many"></a>
#### manyToMany

Defines a `many-to-many` relation. If the other side of the relation is not defined, the
current entity will be the owning side. If both sides are defined, one of them must be defined
as owner and the other as inverse side of the relation.

In this type of relation, only the owner of the relation can have new related entities cascade
through it. Be aware of this limitation and try your best to map only unidirectional `many-to-many`
associations. This will reduce complexity and prevent hard to find bugs.

```php
// Assumes a 'degrees' field will hold the Collection
$builder->manyToMany(Degree::class);

// Uses the 'colleagues' field
$builder->manyToMany(Scientist::class, 'colleagues');
```

<a name="relations-belongs-to-many"></a>
#### belongsToMany

Inspired by Laravel's relation syntax, this is an alias of `$builder->manyToMany()`.

```php
// Assumes a 'degrees' field will hold the Collection
$builder->belongsToMany(Degree::class);

// Uses the 'colleagues' field
$builder->belongsToMany(Scientist::class, 'colleagues');
```


### Relation fetch mode

Doctrine allows mapped relations to have a default fetch behavior. Although this may seem useful, most of the times the
default lazy-loading fetch mode works best for most scenarios, and join queries can be used for the few advanced
requirements.

Although the examples will be using a few of them, the following methods are available on all `Relation` objects.

#### Lazy loading (default)

Wait until the relation is accessed to fetch it from database.

```php
$builder->hasMany(Theory::class)->fetchLazy();
```

#### Eager loading

Always joins the current entity with this relation when fetching it.

```php
$builder->belongsTo(Scientist::class, 'mentor')->fetchEager();
```

#### Extra-Lazy loading

Allow some methods to be _even lazier_ than the lazy loading mode.

See [the doctrine Documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/tutorials/extra-lazy-associations.html)
for a complete explanation and usage examples.

```php
$builder->manyToMany(Scientist::class, 'colleagues')->fetchExtraLazy();
```

### Relation cascading

Doctrine can be configured to cascade operations that happen to the current entity to its relations. This transitivity
is useful for encapsulating knowledge of change inside one entity without forcing the user to traverse all modified
relations.

See [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/working-with-associations.html#transitive-persistence-cascade-operations)
for a complete usage guide.

In the following examples, assume the `$relation` variable holds the result of calling one of the relation methods
described above, e.g.: `$relation = $builder->hasMany(Theory::class)`. This is *not* limited to `*-to-many` relations.

| Method                         | Description                                           |
|--------------------------------|-------------------------------------------------------|
| `$relation->cascadeAll();`     | Configure the relation to cascade all operations.     |
| `$relation->cascadePersist();` | Configure the relation to cascade persist operations. |
| `$relation->cascadeRemove();`  | Configure the relation to cascade remove operations.  |
| `$relation->cascadeMerge();`   | Configure the relation to cascade merge operations.   |
| `$relation->cascadeDetach();`  | Configure the relation to cascade detach operations.  |
| `$relation->cascadeRefresh();` | Configure the relation to cascade refresh operations. |

### Orphan removal

In `one-to-*` and `many-to-many` associations, sometimes removing an entity from a relation means the removed entity
should be removed from the system. This can be acomplished by the `orphan removal` feature.

See an example in [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/working-with-associations.html#orphan-removal).

```php
$builder->oneToMany(Theory::class)->orphanRemoval();
```

*IMPORTANT*: Doctrine will *NOT* check if the removed entity is referred to from anywhere else. Only use orphan removal
when the contained entity is solely referred to from the current entity.

<a name="embeddables"></a>
## Embeddables

Embeddables have to be mapped independently of the entity that uses them. Each embeddable mapping class needs to extend
`LaravelDoctrine\Fluent\EmbeddableMapping`.

To use an embeddable in your entity, the mapping configuration is very similar to a relation:

```php
// Assumes an 'address' field will hold the embedded object
$builder->embed(Address::class);

// Uses the 'email' field
$builder->embed(EmailAddress::class, 'email');
```

Fields declared inside an embedded will have its columns prefixed by the entity's field name, according to the current
`NamingStrategy`. If you wish to change or remove it:

```php
// Change the default deduced prefix ("address_") to "location_"
$builder->embed(Address::class)->prefix('location_');

// Do not prefix columns
$builder->embed(EmailAddress::class, 'email')->noPrefix();
```

<a name="inheritance"></a>
## Inheritance mapping

A family of entities can be mapped to the database as an inheritance structure. Currently
Doctrine supports 3 types of inheritance: *Mapped super classes*, *single table* and *joined table inheritance*

For more information on usage and trade-offs, [see the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/inheritance-mapping.html).

<a name="inheritance-mapped-super-class"></a>
## Mapped super classes

In mapped super class mode, the root class have to be mapped by extending the `LaravelDoctrine\Fluent\MappedSuperClassMapping`
mapping abstract class. This class *will not be an entity*, that means it will not be queryable and
can only have unidirectional relations defined on it.

In this mode, each descendant of the root class will have its own table containing all combined
fields of the parent and child classes.

```php
class Parent {}
class AChild extends Parent {}
class AnotherChild extends Parent {}

class ParentMapping extends MappedSuperClassMapping
{
    public function mapFor()
    {
        return Parent::class;
    }

    public function map()
    {
        // Map as usual...
    }
}

class AChildMapping extends EntityMapping
{
    public function mapFor()
    {
        return AChild::class;
    }

    public function map()
    {
        // No need to add any reference to Parent.
    }
}

class AnotherChildMapping extends EntityMapping
{
    public function mapFor()
    {
        return AnotherChild::class;
    }

    public function map()
    {
        // No need to add any reference to Parent.
    }
}

// Resulting SQL
CREATE TABLE a_children ...
CREATE TABLE another_children ...
```

<a name="inheritance-single"></a>
## Single table inheritance

Defines the current entity as the root of a single table inheritance tree.

In single table inheritance mode, all entities involved (abstract or otherwise) have to be
mapped using the `LaravelDoctrine\Fluent\EntityMapping` mapping abstract class.

The root entity of the inheritance chain will have the `inheritance` mapping, while Doctrine
will figure out its descendants without the need for you to tell it.

In this mode, a single table will hold all children entities, and a `type` column will be used
to determine which children the row represents.

```php
$builder->singleTableInheritance()->column('type');
```

<a name="inheritance-joined"></a>
## Joined table inheritance

Defines the current entity as the root of a joined table inheritance tree.

In joined table inheritance mode, all entities involved (abstract or otherwise) have to be
mapped using the `LaravelDoctrine\Fluent\EntityMapping` mapping abstract class.

The root entity of the inheritance chain will have the `inheritance` mapping, while Doctrine
will figure out its descendants without the need for you to tell it.

While the previous conditions are identical for single and joined table inheritance, the result
is quite different:

In this mode, a table will be created for each entity, including the root. Doctrine will use joins
to fetch the corresponding entity as a result of the root table, the `type` column defined in it and
the data joined from the related table.

```php
$builder->joinedTableInheritance()->column('type');
```

<a name="lifecycle-callbacks"></a>
## Lifecycle callbacks

Doctrine allows configuring specific methods of an entity to be called as listeners of
_lifecycle events_. This lets you easily hook into specific moments of the Doctrine entity lifecycle.

Use `$builder->events()` to access the `LaravelDoctrine\Fluent\Builders\LifecycleEvents` object.

The following documentation is copied from [Doctrine's documentation on lifecycle events](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/events.html#lifecycle-events).

| Method                                                        | Description |
|---------------------------------------------------------------|-------------|
| `$builder->events()->preRemove(string $method)`               | The preRemove event occurs for a given entity before the respective EntityManager remove operation for that entity is executed. It is not called for a DQL DELETE statement.
| `$builder->events()->postRemove(string $method)`              | The postRemove event occurs for an entity after the entity has been deleted. It will be invoked after the database delete operations. It is not called for a DQL DELETE statement.
| `$builder->events()->prePersist(string $method)`              | The prePersist event occurs for a given entity before the respective EntityManager persist operation for that entity is executed. It should be noted that this event is only triggered on initial persist of an entity (i.e. it does not trigger on future updates).
| `$builder->events()->postPersist(string $method)`             | The postPersist event occurs for an entity after the entity has been made persistent. It will be invoked after the database insert operations. Generated primary key values are available in the postPersist event.
| `$builder->events()->preUpdate(string $method)`               | The preUpdate event occurs before the database update operations to entity data. It is not called for a DQL UPDATE statement nor when the computed changeset is empty.
| `$builder->events()->postUpdate(string $method)`              | The postUpdate event occurs after the database update operations to entity data. It is not called for a DQL UPDATE statement.
| `$builder->events()->postLoad(string $method)`                | The postLoad event occurs for an entity after the entity has been loaded into the current EntityManager from the database or after the refresh operation has been applied to it.
| `$builder->events()->loadClassMetadata(string $method)`       | The loadClassMetadata event occurs after the mapping metadata for a class has been loaded from a mapping source. This event is not a lifecycle callback.
| `$builder->events()->onClassMetadataNotFound(string $method)` | Loading class metadata for a particular requested class name failed. Manipulating the given event args instance allows providing fallback metadata even when no actual metadata exists or could be found. This event is not a lifecycle callback.
| `$builder->events()->preFlush(string $method)`                | The preFlush event occurs at the very beginning of a flush operation. This event is not a lifecycle callback.
| `$builder->events()->onFlush(string $method)`                 | The onFlush event occurs after the change-sets of all managed entities are computed. This event is not a lifecycle callback.
| `$builder->events()->postFlush(string $method)`               | The postFlush event occurs at the end of a flush operation. This event is not a lifecycle callback.
| `$builder->events()->onClear(string $method)`                 | The onClear event occurs when the EntityManager#clear() operation is invoked, after all references to entities have been removed from the unit of work. This event is not a lifecycle callback.

<a name="primary-keys"></a>
## Primary keys

Primary keys can be defined on a field-by-field basis by calling the `primary()` method on the `Field` object.
Also, owning side of `*ToOne` relations can also be marked as primary with the same syntax.

```php
$builder->binary('id')->length(16)->primary();
$builder->belongsTo(Foo::class)->primary();
```

If you prefer to have a single declaration of a composite key, you can use the builder to define which columns will be
used as primary key.

```php
$builder->primary(['id', 'foo_id']);
```

> Although Doctrine provides multiple ways of declaring primary keys, most often the combination of a single primary key
(either auto-generated or application-based, such as UUIDs) and a unique constraint is enough to satisfy data restrictions
while also maintaining flexibility and simplifying use. Take this into consideration when using composite primary keys.

<a name="indexes"></a>
## Indexes

Indexes can be created on a field-by-field basis by calling the `index()` method on the `Field` object.

```php
$builder->string('slug')->index();
```

Indexes that span more than one column have to be created separately from the field definition:

```php
$builder->index(['theory_id', 'publication_id']);
```

<a name="uniques"></a>
## Unique Constraints

Unique constraints can be created on a field-by-field basis by calling the `unique()` method on the `Field` object.

```php
$builder->string('slug')->unique();
```

Unique constraints that span more than one column have to be created separately from the field definition:

```php
$builder->unique(['scientist_id', 'slug']);
```

<a name="overrides"></a>
## Mapping overrides

Sometimes using external libraries can bring undesired mapping configurations into our entities. This is mostly the case
when extending or using traits from external sources.

You can override this mapping configurations by using the `override` method:

```php
// Field override
$builder->override('name', function(Field $field){
    $field->length(60);
});

// Relation override
$builder->override('theories', function(OneToMany $relation){
    $relation->cascadePersist();
});
```

In this case, the `callable $callback` parameter is *required*, as it's your only way to obtain the mapping object you
want to edit. This `callable` will receive an instance of either `Field` or a specific `Relation`, depending on what
was already mapped in the given `$name`.

<a name="macros"></a>
## Extending with macros

You can extend the `Builder` object with custom macros.

To extend the builder, call the `extend` function while booting your application:

```php
LaravelDoctrine\Fluent\Builders\Builder::extend('timestamps', function(Fluent $builder){
    // Use the builder to declare your custom macro
    $createdAt = $builder->carbonDateTime('createdAt');
    $updatedAt = $builder->carbonDateTime('updatedAt');

    // Anything you return will be returned back at you while using this macro
    return [$createdAt, $updatedAt];
});
```

Then, to use this macro, you can call it on the `$builder` instance and it will use the magic `__call` method to resolve:

```php
public function map(Fluent $builder)
{
    list($createdAt, $updatedAt) = $builder->timestamps();
}
```

<a name="advanced"></a>
## Advanced Doctrine Entity configuration

You can access advanced configuration through the `$builder->entity()` method.
The returned `LaravelDoctrine\Fluent\Builders\Entity` object will have the following methods:

```php
$builder->entity()->setRepositoryClass(DoctrineScientistRepository::class);
```

Set a custom repository class, to be used when called from `EntityManager::getRepository(string $entityName)`.

If you are mapping custom repositories to a DI Container and resolving them by injection, then you probably don't need this.

```php
$builder->entity()->readOnly();
```

Mark this entity as *read only*. See [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/annotations-reference.html#entity).

```php
$builder->entity()->cacheable(ClassMetadataInfo::CACHE_USAGE_NONSTRICT_READ_WRITE, 'scientists_cache_region');
```

Mark this entity as cacheable, using the `Second Level Cache` functionality.

Read more about second level cache usage and regions [in the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/second-level-cache.html).