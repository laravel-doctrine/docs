# Timestamps

Timestamps allows you to automatically record the time of certain events against your entities. This can be used
to provide similar behaviour to the timestamps feature in Laravel's Eloquent ORM.

Features:

* Automatic predefined date field update on creation, update, property subset update, and even on record property changes
ORM and ODM support using same listener
* Specific annotations for properties, and no interface required
* Can react to specific property or relation changes to specific value
* Can be nested with other behaviors
* Annotation, Yaml and Xml mapping support for extensions

#### Adding `created_at` and `updated_at` to Your Entities

This package provides a trait to automatically store the `created_at` and `updated_at` time of your entities.

Add the following to the entity you wish to track `created_at` and `updated_at` on

```php

use LaravelDoctrine\Extensions\Timestamps;

class Entity {

    use Timestamps;

    //

}
```

For full documentation see [here.](https://github.com/Atlantic18/DoctrineExtensions/blob/master/doc/timestampable.md)