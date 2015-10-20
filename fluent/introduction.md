# Introduction

#### A fluent mapping driver for Doctrine2

This mapping driver allows you to manage your mappings in an Object Oriented approach, separating your entities
from your mapping configuration without the need for configuration files like XML or YAML.
This is done by implementing the `LaravelDoctrine\Fluent\Mapping` interface, or extending the abstract classes
provided with this package for an easier use:
`LaravelDoctrine\Fluent\EntityMapping`, `LaravelDoctrine\Fluent\EmbeddableMapping` or `MappedSuperClassMapping`.


This package provides a fluent Builder over Doctrine's `ClassMetadataBuilder`, aimed at easing usage of
Doctrine's mapping concepts in Laravel. The builder adds syntax sugar and implements the same grammar that you
might use in Laravel migrations.

```
use LaravelDoctrine\Fluent\EntityMapping;

class UserMapping extends EntityMapping
{
    /**
     * Returns the fully qualified name of the class that this mapper maps.
     *
     * @return string
     */
    public function mapFor()
    {
        return \App\User::class;
    }

    /**
     * Load the object's metadata through the Metadata Builder object.
     *
     * @param Fluent $builder
     */
    public function map(Fluent $builder)
    {
        $builder->increments('id');
        $builder->embed(App\Email::class, 'email');
        $builder->string('password')->length(60);
        $builder->boolean('activated')->default(false);

        // Laravel's grammar
        $builder->belongsToMany(Role::class, 'roles')->owns('users');
        $builder->hasOne(Profile::class, 'profile')->ownedBy('user');

        // Doctrine's grammar
        $builder->oneToMany(Address::class, 'addresses')->mappedBy('user');
        $builder->manyToOne(Tenant::class, 'tenant')->inversedBy('users');

        // Can be mixed up!
        $builder->hasMany(Category::class, 'categories')->mappedBy('users');
    }
}
```
