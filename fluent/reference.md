# Mapping Reference

This is the complete Fluent mapping reference.

- [Table](#table)
- [Columns](#columns)
- [Relations](#relations)
- [Embeddables](#embeddables)
- [Inheritance mapping](#inheritance)
- [Lifecycle callbacks](#lifecycle-callbacks)
- [Primary keys](#primary-keys)
- [Indexes](#indexes)
- [Unique Constraints](#uniques)
- [Mapping overrides](#overrides)
- [Extending with macros](#macros)
- [Advanced Doctrine Entity configuration](#advanced)

## Table <a name="table"></a>

Each `Entity` defined in Doctrine will have a table associated with it. The name of the table is automatically
generated for you through a `NamingStrategy`, but you can change it if you need to:

```php
table(string $name, callable $callback = null): LaravelDoctrine\Fluent\Builders\Table
```

## Columns <a name="columns"></a>
Columns are defined through their type and associated field. In Doctrine, we define fields in our
entities and then create a database schema based on them, with a given `NamingStrategy`.

All column methods return an instance of `LaravelDoctrine\Fluent\Builders\Field` and, if the user
prefers, an optional callback can be used instead of the _fluent_ interface. This callback will
receive the same `LaravelDoctrine\Fluent\Builders\Field` instance as the only parameter.

*IMPORTANT*: All `$name` variables used here expect the *field name*. That means the name of the property inside your
 Entity, *not the column name you expect to have in your database*. Also, *fields have to be explicitly declared in the
 Entity class*.

### Type-based fields

```php
string(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a string.

```php
text(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as text.

```php
integer(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an integer.

```php
smallInteger(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a small integer.

```php
bigInteger(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a big integer.

```php
unsignedInteger(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an unsigned integer.

This is an alias of `$builder->integer($name)->unsigned()`.

```php
unsignedSmallInteger(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an unsigned small integer.

This is an alias of `$builder->smallInteger($name)->unsigned()`.

```php
unsignedBigInteger(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an unsigned big integer.

This is an alias of `$builder->bigInteger($name)->unsigned()`.

```php
float(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a floating point number.

```php
decimal(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a decimal number.

```php
boolean(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a boolean.

```php
binary(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a binary.

```php
jsonArray(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a JSON array.

```php
guid(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a GUID.

```php
blob(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a *b*inary *l*arge *ob*ject

```php
object(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an object.

```php
setArray(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as an array.

```php
array(string $name, callable $callback = null): Field
```

This is an alias of `$builder->setArray($name)`;

```php
simpleArray(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a simple array.

```php
field(string $type, string $name, callable $callback = null): Field
```

Generic method for adding any type of field that Doctrine supports.
You can find type constants in `Doctrine\DBAL\Types\Type`, or use your own.

### Date columns

```php
date(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a date, using the native `DateTime` object.

```php
dateTime(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a datetime, using the native `DateTime` object.

```php
dateTimeTz(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a datetime with timezone, using the native `DateTime` object.

```php
time(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a time, using the native `DateTime` object.

```php
carbonDate(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a date, using the `Carbon\Carbon` object.

```php
carbonDateTime(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a datetime, using the `Carbon\Carbon` object.

```php
carbonDateTimeTz(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a datetime with timezone, using the `Carbon\Carbon` object.

```php
carbonTime(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a time, using the `Carbon\Carbon` object.

```php
zendDate(string $name, callable $callback = null): Field
```

Defines a field with the given `$name` as a datetime, using the `Zend_Date` object.

```php
timestamp(string $name, callable $callback = null): Field
```

Alias of `$builder->carbonDateTime($name, $callback)`.

```php
timestampTz(string $name, callable $callback = null): Field
```

Alias of `$builder->carbonDateTimeTz($name, $callback)`.

### Helper methods

Fluent provides helper methods for frequently used functionality.

```php
rememberToken(string $name = 'rememberToken', callable $callback = null): Field
```

Adds a `string` field compatible with Laravel's "remember me" functionality.

```php
increments(string $name, callable $callback = null): Field
```

Adds an unsigned integer with an auto-increment generated value.

```php
smallIncrements(string $name, callable $callback = null): Field
```

Adds a small unsigned integer with an auto-increment generated value.

```php
bigIncrements(string $name, callable $callback = null): Field
```

Adds a big unsigned integer with an auto-increment generated value.

### Field customizations

All fields can be customized with the following methods, taken from [the Doctrine Annotations documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/annotations-reference.html#column),
as it implements the same features:

```php
unique(boolean $flag = true): Field
```

Boolean value to determine if the value of the column should be unique across all rows of the underlying entities
table.

```php
nullable(boolean $flag = true): Field
```

Determines if NULL values are allowed for this column.

```php
length(int $length): Field
```

Used by the “string” type to determine its maximum length in the database. Doctrine does not validate the length of
string values for you.

```php
columnName(string $column): Field
```

By default the property name is used for the database column name also, however the ‘name’ attribute allows you to
determine the column name.

```php
name(string $columnName): Field
```

Alias of `$field->columnName($columnName)`

```php
precision(int $precision): Field
```

The precision for a decimal (exact numeric) column (applies only for decimal column), which is the maximum number
of digits that are stored for the values.

```php
scale(int $scale): Field
```

The scale for a decimal (exact numeric) column (applies only for decimal column), which represents the number of
digits to the right of the decimal point and must not be greater than precision.

```php
default(string $default): Field
```

The default value to set for the column if no value is supplied.

```php
columnDefinition(string $def): Field
```

DDL SQL snippet that starts after the column name and specifies the complete (non-portable!) column definition.
This attribute allows to make use of advanced RMDBS features. However you should make careful use of this feature
and the consequences. SchemaTool will not detect changes on the column correctly anymore if you use “columnDefinition”.

```php
option($name, $value): Field
```

Set custom options

```php
unsigned(): Field
```

Set the current field (must be numeric) to `unsigned`.

```php
setDefault($default): Field
```

Set the default value of the column. Although this may seem useful, default values can be more flexible by setting
them on entities.

```php
default($default): Field
```

Alias of `setDefault($default)`.

```php
fixed(boolean $fixed): Field
```

Boolean value to determine if the specified length of a string column should be fixed or varying (applies only for
string/binary column and might not be supported by all vendors).

```php
comment(string $comment): Field
```

The comment of the column in the schema (might not be supported by all vendors).

```php
collation(string $collation): Field
```

The collation of the column (only supported by Drizzle, Mysql, PostgreSQL>=9.1, Sqlite and SQLServer).

```php
useForVersioning(): Field
```

Defines the specified column as version attribute used in an optimistic locking scenario.
It only works on integer or datetime columns. Combining `useForVersioning()` with `primary()` is not supported.

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
auto(string $name = null, int $initial = null, int $size = null): GeneratedValue
```

Tells Doctrine to pick the strategy that is preferred by the used database platform. The preferred strategies
are IDENTITY for MySQL, SQLite, MsSQL and SQL Anywhere and SEQUENCE for Oracle and PostgreSQL. This strategy
provides full portability. This is the default strategy.

If parameters are given, they will be used to configure the `sequenceGenerator`.

```php
sequence(string $name = null, int $initial = null, int $size = null): GeneratedValue
```

Tells Doctrine to use a database sequence for ID generation. This strategy does currently not provide full
portability. Sequences are supported by Oracle, PostgreSql and SQL Anywhere.

If parameters are given, they will be used to configure the `sequenceGenerator`.

```php
identity(): GeneratedValue
```

Tells Doctrine to use special identity columns in the database that generate a value on insertion of a row.
This strategy does currently not provide full portability and is supported by the following platforms:

MySQL/SQLite/SQL Anywhere (AUTO_INCREMENT), MSSQL (IDENTITY) and PostgreSQL (SERIAL).

```php
uuid(): GeneratedValue
```

Tells Doctrine to use the built-in Universally Unique Identifier generator.
This strategy provides full portability.

*NOTE*: This UUIDs will be generated on the database. To use application-side UUIDs, you should use either a `guid()`
field (PostgreSQL) or a `binary()->length(16)` field (MySQL), and create / receive the corresponding UUID on your
Entity's `__construct()` method.

```php
none(): GeneratedValue
```

Tells Doctrine that the identifiers are assigned (and thus generated) by your code. The assignment must take
place before a new entity is passed to EntityManager#persist.

NONE is the same as leaving off the GeneratedValue entirely.

```php
custom(string $generatorClass): GeneratedValue
```

Tells Doctrine to use a custom Generator class to generate identifiers.
The given class must extend `Doctrine\ORM\Id\AbstractIdGenerator`

## Relations <a name="relations"></a>

Relations are defined associating the current entity with another entity by its fully qualified
class name. All relations will return a specific `LaravelDoctrine\Fluent\Relations\Relation` child object
that can be used to further define it.

The relation field can be deduced by the driver based on the referenced class name, using singular or
plural depending on the relation type, so all `$field` parameters are optional.

This driver also provides alias methods to have Laravel's syntax in Doctrine.

```php
hasOne(string $entity, string $field = null, callable $callback = null): OneToOne
```

This method maps a `one-to-one` inverse side relation. This means that the current entity does not
hold the foreign key, but is referenced by the given `$entity`. As the relation is a `one-to-one`, a
unique constraint will be added on the owning side to prevent multiple objects being related to this one.

By default, the current entity will hold a proxy instance of the related entity until it is accessed, unless
an `eager loading` fetch mode is configured.

*IMPORTANT*: This is *NOT* an alias of `oneToOne($entity, $field, $callback)`. It is closer to a version of
`oneToMany`, restricted to only one related entity on the inverse side by a `unique` constraint.

```php
oneToOne(string $entity, string $field = null, callable $callback = null): OneToOne
```

Define a `one-to-one` relation. If not defined, the current entity will be the owning side of the relation
(that means it will be the one holding the foreign key.)

```php
belongsTo(string $entity, string $field = null, callable $callback = null): ManyToOne
```

This is an alias of `$builder->manyToOne($entity, $field, $callback)`

```php
manyToOne(string $entity, string $field = null, callable $callback = null): ManyToOne
```

Define the owning side of a `many-to-one` relation (that means it will be the one holding the foreign key.)

```php
hasMany(string $entity, string $field = null, callable $callback = null): OneToMany
```

This is an alias of `$builder->oneToMany($entity, $field, $callback)`.

```php
oneToMany(string $entity, string $field = null, callable $callback = null): OneToMany
```

Define the inverse side of a `one-to-many` relation.

The `one-to-many` relations in Doctrine cannot be defined as unidirectional from the inverse side
without using a join table. To do so, you have to define a `many-to-many` relation and set the joined
column as unique:

```php
// you can either...
$builder->manyToMany(Foo::class, 'foo')
    ->joinColumn('foo_id', 'bar_id', true);

// or...
$builder->belongsToMany(Foo::class)
    ->joinColumn('foo_id', 'bar_id', true);
```

```php
manyToMany(string $entity, $field, callable $callback = null): ManyToMany
```

Define a `many-to-many` relation. If the other side of the relation is not defined, the
current entity will be the owning side. If both sides are defined, one of them must be defined
as owner and the other as inverse side of the relation.

In this type of relation, only the owner of the relation can have new related entities cascade
through it. Be aware of this limitation and try your best to map only unidirectional `many-to-many`
associations. This will reduce complexity and prevent hard to find bugs.

```php
belongsToMany(string $entity, $field = null, callable $callback = null): ManyToMany
```

This is an alias of `$builder->manyToMany()`.

```php
addRelation(Relation $relation, callable $callback = null): Relation
```

Generic method for adding any type of relation that Doctrine supports.

This is mostly internal, but exposed for future compatibility.

### Relation fetch mode

Doctrine allows mapped relations to have a default fetch behavior. Although this may seem useful, most of the times the
default lazy-loading fetch mode works best for most scenarios, and join queries can be used for the few advanced
requirements.

The following methods are available on all `Relation` objects.

#### Lazy loading (default)

Wait until the relation is accessed to fetch it from database.

```php
fetchLazy(): Relation;
```

#### Eager loading

Always joins the current entity with this relation when fetching it.

```php
fetchEager(): Relation;
```

#### Extra-Lazy loading

Allow some methods to be even lazier than the lazy loading mode.

See [the doctrine Documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/tutorials/extra-lazy-associations.html)
for a complete explanation and usage examples.

```php
fetchLazy(): Relation;
```

### Relation cascading

Doctrine can be configured to cascade operations that happen to the current entity to its relations. This transitivity
is useful for encapsulating knowledge of change inside one entity without forcing the user to traverse all modified
relations.

See [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/working-with-associations.html#transitive-persistence-cascade-operations)
for a complete usage guide.

```php
cascadeAll(): Relation
```

Configure the relation to cascade all operations.

```php
cascadePersist(): Relation
```

Configure the relation to cascade persist operations.

```php
cascadeRemove(): Relation
```

Configure the relation to cascade remove operations.

```php
cascadeMerge(): Relation
```

Configure the relation to cascade merge operations.

```php
cascadeDetach(): Relation
```

Configure the relation to cascade detach operations.

```php
cascadeRefresh(): Relation
```

Configure the relation to cascade refresh operations.

### Orphan removal

In `one-to-*` and `many-to-many` associations, sometimes removing an entity from a relation means the removed entity
should be removed from the system. This can be acomplished by the `orphan removal` feature.

See an example in [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/working-with-associations.html#orphan-removal).

```php
orphanRemoval(): Relation
```

*IMPORTANT*: Doctrine will *NOT* check if the removed entity is referred to from anywhere else. Only use orphan removal
when the contained entity is solely referred to from the current entity.

## Embeddables <a name="embeddables"></a>

Embeddables have to be mapped independently of the entity that uses them. Each embeddable mapping class needs to extend
`LaravelDoctrine\Fluent\EmbeddableMapping`.

To use an embeddable in your entity, the mapping configuration is very similar to a relation:

```php
embed(string $embeddable, string $field = null, callable $callback = null): LaravelDoctrine\Fluent\Builders\Embedded
```

Fields declared inside an embedded will have its columns prefixed by the entity's field name, according to the current
`NamingStrategy`. If you wish to change or remove it, you can do so by calling `prefix($prefix)` or `noPrefix()` on the
`Embedded` object.

## Inheritance mapping <a name="inheritance"></a>

A family of entities can be mapped to the database as an inheritance structure. Currently
Doctrine supports 3 types of inheritance:

-   *Mapped Superclasses*

    In mapped superclass mode, the root class have to be mapped by extending the `LaravelDoctrine\Fluent\MappedSuperClassMapping`
    mapping abstract class. This class *will not be an entity*, that means it will not be queryable and
    can only have unidirectional relations defined on it.

    In this mode, each descendant of the root class will have its own table containing all combined
    fields of the parent and child classes.
-   *Single table inheritance*

    In single table inheritance mode, all entities involved (abstract or otherwise) have to be
    mapped using the `LaravelDoctrine\Fluent\EntityMapping` mapping abstract class.

    The root entity of the inheritance chain will have the `inheritance` mapping, while Doctrine
    will figure out its descendants without the need for you to tell it.

    In this mode, a single table will hold all children entities, and a `type` column will be used
    to determine which children the row represents.
-   *Joined table inheritance*

    In joined table inheritance mode, all entities involved (abstract or otherwise) have to be
    mapped using the `LaravelDoctrine\Fluent\EntityMapping` mapping abstract class.

    The root entity of the inheritance chain will have the `inheritance` mapping, while Doctrine
    will figure out its descendants without the need for you to tell it.

    While the previous conditions are identical for single and joined table inheritance, the result
    is quite different:

    In this mode, a table will be created for each entity, including the root. Doctrine will use joins
    to fetch the corresponding entity as a result of the root table, the `type` column defined in it and
    the data joined from the related table.

For more information on usage and trade-offs, [see the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/inheritance-mapping.html).

```php
singleTableInheritance(callable $callback = null): LaravelDoctrine\Fluent\Builders\Inheritance
```

Defines the current entity as the root of a single table inheritance tree.

```php
joinedTableInheritance(callable $callback = null): LaravelDoctrine\Fluent\Builders\Inheritance
```

Defines the current entity as the root of a joined table inheritance tree.

```php
inheritance($type, callable $callback = null): LaravelDoctrine\Fluent\Builders\Inheritance
```

Generic method for adding any type of relation that Doctrine supports.

## Lifecycle callbacks <a name="lifecycle-callbacks"></a>

Doctrine allows configuring specific methods of an entity to be called as listeners of
_lifecycle events_. This lets you easily hook into specific moments of the Doctrine entity lifecycle.

Use `$builder->events()` to access the `LaravelDoctrine\Fluent\Builders\LifecycleEvents` object.

The following documentation is copied from [Doctrine's documentation on lifecycle events](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/events.html#lifecycle-events).

```php
preRemove(string $method): LifecycleEvents
```
The preRemove event occurs for a given entity before the respective EntityManager remove operation for that entity
is executed. It is not called for a DQL DELETE statement.

```php
postRemove(string $method): LifecycleEvents
```
The postRemove event occurs for an entity after the entity has been deleted. It will be invoked after the database
delete operations. It is not called for a DQL DELETE statement.

```php
prePersist(string $method): LifecycleEvents
```
The prePersist event occurs for a given entity before the respective EntityManager persist operation for that entity
is executed. It should be noted that this event is only triggered on initial persist of an entity (i.e. it does not
trigger on future updates).

```php
postPersist(string $method): LifecycleEvents
```
The postPersist event occurs for an entity after the entity has been made persistent. It will be invoked after the
database insert operations. Generated primary key values are available in the postPersist event.

```php
preUpdate(string $method): LifecycleEvents
```
The preUpdate event occurs before the database update operations to entity data. It is not called for a DQL UPDATE
statement nor when the computed changeset is empty.

```php
postUpdate(string $method): LifecycleEvents
```
The postUpdate event occurs after the database update operations to entity data. It is not called for a DQL UPDATE
statement.

```php
postLoad(string $method): LifecycleEvents
```
The postLoad event occurs for an entity after the entity has been loaded into the current EntityManager from the
database or after the refresh operation has been applied to it.

```php
loadClassMetadata(string $method): LifecycleEvents
```
The loadClassMetadata event occurs after the mapping metadata for a class has been loaded from a mapping source.
This event is not a lifecycle callback.

```php
onClassMetadataNotFound(string $method): LifecycleEvents
```
Loading class metadata for a particular requested class name failed. Manipulating the given event args instance
allows providing fallback metadata even when no actual metadata exists or could be found. This event is not a
lifecycle callback.

```php
preFlush(string $method): LifecycleEvents
```
The preFlush event occurs at the very beginning of a flush operation. This event is not a lifecycle callback.

```php
onFlush(string $method): LifecycleEvents
```
The onFlush event occurs after the change-sets of all managed entities are computed. This event is not a lifecycle
callback.

```php
postFlush(string $method): LifecycleEvents
```
The postFlush event occurs at the end of a flush operation. This event is not a lifecycle callback.

```php
onClear(string $method): LifecycleEvents
```
The onClear event occurs when the EntityManager#clear() operation is invoked, after all references to entities
have been removed from the unit of work. This event is not a lifecycle callback.

## Primary keys <a name="primary-keys"></a>

Primary keys can be defined on a field-by-field basis by calling the `primary()` method on the `Field` object.
Also, owning side of `*ToOne` relations can also be marked as primary with the same syntax.

If you prefer to have a single declaration of a composite key, you can use the builder to define which columns will be
used as primary key.

```php
primary(array|string $fields): LaravelDoctrine\Fluent\Builders\Primary
```

> Although Doctrine provides multiple ways of declaring primary keys, most often the combination of a single primary key
(either auto-generated or application-based, such as UUIDs) and a unique constraint is enough to satisfy data restrictions
while also maintaining flexibility and simplifying use. Take this into consideration when using composite primary keys.

## Indexes <a name="indexes"></a>

Indexes can be created on a field-by-field basis by calling the `index()` method on the `Field` object.

Indexes that span more than one column have to be created separately from the field definition:

```php
index(array|string $columns): \LaravelDoctrine\Fluent\Builders\Index
```

## Unique Constraints <a name="uniques"></a>

Unique constraints can be created on a field-by-field basis by calling the `unique()` method on the `Field` object.

Unique constraints that span more than one column have to be created separately from the field definition:

```php
unique(array|string $columns): \LaravelDoctrine\Fluent\Builders\UniqueConstraint
```

## Mapping overrides <a name="overrides"></a>

Sometimes using external libraries can bring undesired mapping configurations into our entities. This is mostly the case
when extending or using traits from external sources.

You can override this mapping configurations by using the `override` method:

```php
override(string $name, callable $callback): LaravelDoctrine\Fluent\Builders\Overrides\Override
```

In this case, the `callable $callback` parameter is *required*, as it's your only way to obtain the mapping object you
want to edit. This `callable` will receive an instance of either `Field` or a specific `Relation`, depending on what
was already mapped in the given `$name`.

## Extending with macros <a name="macros"></a>

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

## Advanced Doctrine Entity configuration <a name="advanced"></a>

You can access advanced configuration through the `$builder->entity()` method.
The returned `LaravelDoctrine\Fluent\Builders\Entity` object will have the following methods:

```php
setRepositoryClass(string $class): Entity
```

Set a custom repository class, to be used when called from `EntityManager::getRepository(string $entityName)`.

If you are mapping custom repositories to a DI Container and resolving them by injection, then you probably don't need this.

```php
readOnly(): Entity
```

Mark this entity as *read only*. See [the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/annotations-reference.html#entity).

```php
cacheable($usage = ClassMetadataInfo::CACHE_USAGE_READ_ONLY, $region = null): Entity
```

Mark this entity as cacheable, using the `Second Level Cache` functionality.

Read more about second level cache usage and regions [in the Doctrine documentation](http://doctrine-orm.readthedocs.org/projects/doctrine-orm/en/latest/reference/second-level-cache.html).