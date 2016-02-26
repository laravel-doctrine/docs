# Extensions


## Gedmo extensions
- [Installation](#installation)
- [Blameable](#blameable)
- [IpTraceable](#iptraceable)
- [Loggable](#loggable)
- [Sluggable](#sluggable)
- [SoftDeletes](#softdeletes)
- [Sortable](#sortable)
- [Timestamps](#timestamps)
- [Translatable](#translatable)
- [Tree](#tree)
- [Uploadable](#uploadable)
 
<a name="installation"></a>
### Installation

To include Gedmo extensions install them:

```
composer require "gedmo/doctrine-extensions=^2.4"
```

#### In Laravel Doctrine

Require the extensions package in composer:

```
composer require "laravel-doctrine/extensions:1.0.*"
```

Add the Gedmo (Behavioral) extensions service provider in `config/app.php`:

```php
LaravelDoctrine\Extensions\GedmoExtensionsServiceProvider::class,
```

#### Standalone usage

To register all Gedmo extension for Fluent and register all abstract mapping files:
```
LaravelDoctrine\Fluent\Extensions\GedmoExtensions::registerAbstract($chain);
```

To register all Gedmo extension for Fluent and register all mapping files:
```
LaravelDoctrine\Fluent\Extensions\GedmoExtensions::registerAll($chain);
```

<a name="blameable"></a>
### Blameable

<a name="iptraceable"></a>
### IpTraceable

<a name="loggable"></a>
### Loggable

<a name="sluggable"></a>
### Sluggable

<a name="softdeletes"></a>
### SoftDeletes

<a name="sortable"></a>
### Sortable

<a name="timestamps"></a>
### Timestamps

<a name="translatable"></a>
### Translatable

<a name="tree"></a>
### Tree

<a name="uploadable"></a>
### Uploadable