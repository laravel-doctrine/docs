# Configuration File Overview

This page provides a quick overview of all of the options provided in `doctrine.php`, the configuration file for Laravel Doctrine.

## Contents

- [Sample Configuration](#sample-configuration)
- [Entity Manager](#entity-manager)
    - [Events and Filters](#events-and-filters)
- [Extensions](#extensions)
- [Custom Types](#custom-types)
- [Custom Functions](#custom-functions)
- [Logger](#logger)
- [Cache](#cache)
- [Gedmo Extensions](#gedmo)

### <a name="sample-configuration"></a> Sample Configuration

```php
<?php

return [

    /*
    |--------------------------------------------------------------------------
    | Entity Mangers
    |--------------------------------------------------------------------------
    |
    | Configure your Entity Managers here. You can set a different connection
    | and driver per manager and configure events and filters. Change the
    | paths setting to the appropriate path and replace App namespace
    | by your own namespace.
    |
    | Available meta drivers: annotations|yaml|xml|config|static_php
    |
    | Available connections: mysql|oracle|pgsql|sqlite|sqlsrv
    | (Connections can be configured in the database config)
    |
    | --> Warning: Proxy auto generation should only be enabled in dev!
    |
    */
    'managers'                  => [
        'default' => [
            'dev'        => env('APP_DEBUG'),
            'meta'       => env('DOCTRINE_METADATA', 'annotations'),
            'connection' => env('DB_CONNECTION', 'mysql'),
            'namespaces' => [
                'App'
            ],
            'paths'      => [
                base_path('app')
            ],
            'repository' => Doctrine\ORM\EntityRepository::class,
            'proxies'    => [
                'namespace'     => false,
                'path'          => storage_path('proxies'),
                'auto_generate' => env('DOCTRINE_PROXY_AUTOGENERATE', false)
            ],
            /*
            |--------------------------------------------------------------------------
            | Doctrine events
            |--------------------------------------------------------------------------
            |
            | The listener array expects the key to be a Doctrine event
            | e.g. Doctrine\ORM\Events::onFlush
            |
            */
            'events'     => [
                'listeners'   => [],
                'subscribers' => []
            ],
            'filters'    => []
        ]
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine Extensions
    |--------------------------------------------------------------------------
    |
    | Enable/disable Doctrine Extensions by adding or removing them from the list
    |
    | If you want to require custom extensions you will have to require
    | laravel-doctrine/extensions in your composer.json
    |
    */
    'extensions'                => [
        //LaravelDoctrine\ORM\Extensions\TablePrefix\TablePrefixExtension::class,
        //LaravelDoctrine\Extensions\Timestamps\TimestampableExtension::class,
        //LaravelDoctrine\Extensions\SoftDeletes\SoftDeleteableExtension::class,
        //LaravelDoctrine\Extensions\Sluggable\SluggableExtension::class,
        //LaravelDoctrine\Extensions\Sortable\SortableExtension::class,
        //LaravelDoctrine\Extensions\Tree\TreeExtension::class,
        //LaravelDoctrine\Extensions\Loggable\LoggableExtension::class,
        //LaravelDoctrine\Extensions\Blameable\BlameableExtension::class,
        //LaravelDoctrine\Extensions\IpTraceable\IpTraceableExtension::class,
        //LaravelDoctrine\Extensions\Translatable\TranslatableExtension::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom types
    |--------------------------------------------------------------------------
    */
    'custom_types'              => [
        'json' => LaravelDoctrine\ORM\Types\Json::class
    ],
    /*
    |--------------------------------------------------------------------------
    | DQL custom datetime functions
    |--------------------------------------------------------------------------
    */
    'custom_datetime_functions' => [],
    /*
    |--------------------------------------------------------------------------
    | DQL custom numeric functions
    |--------------------------------------------------------------------------
    */
    'custom_numeric_functions'  => [],
    /*
    |--------------------------------------------------------------------------
    | DQL custom string functions
    |--------------------------------------------------------------------------
    */
    'custom_string_functions'   => [],
    /*
    |--------------------------------------------------------------------------
    | Enable query logging with laravel file logging,
    | debugbar, clockwork or an own implementation.
    | Setting it to false, will disable logging
    |
    | Available:
    | - LaravelDoctrine\ORM\Loggers\LaravelDebugbarLogger
    | - LaravelDoctrine\ORM\Loggers\ClockworkLogger
    | - LaravelDoctrine\ORM\Loggers\FileLogger
    |--------------------------------------------------------------------------
    */
    'logger'                    => env('DOCTRINE_LOGGER', false),
    /*
    |--------------------------------------------------------------------------
    | Cache
    |--------------------------------------------------------------------------
    |
    | Configure meta-data, query and result caching here.
    | Optionally you can enable second level caching.
    |
    | Available: acp|array|file|memcached|redis
    |
    */
    'cache'                     => [
        'default'                => env('DOCTRINE_CACHE', 'array'),
        'namespace'              => null,
        'second_level'           => false,
    ],
    /*
    |--------------------------------------------------------------------------
    | Gedmo extensions
    |--------------------------------------------------------------------------
    |
    | Settings for Gedmo extensions
    | If you want to use this you will have to require
    | laravel-doctrine/extensions in your composer.json
    |
    */
    'gedmo'                     => [
        'all_mappings' => false
    ]
];
```

### <a name="entity-manager"></a> Entity Manager

An **Entity Manager (EM)** contains all of the information Doctrine needs to understand, retrieve, and manipulate a set of entities (models). 

You must have **at least one** EM in order to use Laravel Doctrine.

To use more than one EM simply create another entry in the `managers` array.

* **EM Name** - In the sample below the EM we have configured is named `default`. This is the EM that Laravel Doctrine will attempt to use if no argument is provided to `ManagerRegistry`.
* **dev** - Whether this EM is in development mode.
* **meta** - The metadata driver to use. Built-in options are `annotations|yaml|xml|config|static_php`
* **connection** - The connection drier to use. Built-in options are `mysql|oracle|pgsql|sqlite|sqlsrv`
* **namespaces** - (Optional) If your entities are not located in the configured app namespace you can specify a different one here.
* **paths** - An paths where the mapping configurations for your entities is located.
* **repository** - (Optional) The default repository to use for this EM.
* **proxies** - (Optional) Configuration settings for proxy classes for your entities.
 * **namespace** - Namespace (if different) specified for proxy classes
 * **path** - The path where proxy classes should be generated.
 * **auto_generate** - Should proxy classes be generated every time an EM is created? (Turn off production)


```php
    'managers'                  => [
        'default' => [
            'dev'        => env('APP_DEBUG'),
            'meta'       => env('DOCTRINE_METADATA', 'annotations'),
            'connection' => env('DB_CONNECTION', 'mysql'),
            'namespaces' => [
                'App'
            ],
            'paths'      => [
                base_path('app')
            ],
            'repository' => Doctrine\ORM\EntityRepository::class,
            'proxies'    => [
                'namespace'     => false,
                'path'          => storage_path('proxies'),
                'auto_generate' => env('DOCTRINE_PROXY_AUTOGENERATE', false)
            ],
            'events' => ...
            'filters' => ...
        ]
    ]
```

### <a name="extensions"></a> Extensions

Extensions can be enabled by adding them to this array. They provide additional functionality Entities (Timestamps, Loggable, etc.)

To use the extensions in this sample you must install the extensions package:

`require laravel-doctrine/extensions`

and follow the [installation instructions.](http://www.laraveldoctrine.org/docs/1.0/extensions/installation)

```php
'extensions'                => [
    //LaravelDoctrine\ORM\Extensions\TablePrefix\TablePrefixExtension::class,
    //LaravelDoctrine\Extensions\Timestamps\TimestampableExtension::class,
    //LaravelDoctrine\Extensions\SoftDeletes\SoftDeleteableExtension::class,
    //LaravelDoctrine\Extensions\Sluggable\SluggableExtension::class,
    //LaravelDoctrine\Extensions\Sortable\SortableExtension::class,
    //LaravelDoctrine\Extensions\Tree\TreeExtension::class,
    //LaravelDoctrine\Extensions\Loggable\LoggableExtension::class,
    //LaravelDoctrine\Extensions\Blameable\BlameableExtension::class,
    //LaravelDoctrine\Extensions\IpTraceable\IpTraceableExtension::class,
    //LaravelDoctrine\Extensions\Translatable\TranslatableExtension::class
],
```

### <a name="custom-types"></a> Custom Types

Custom types are classes that allow Doctrine to marshal data to/from the data source in a custom format. 

To register a custom type simple add the class to this list. [For more information on custom types refer to the Doctrine documentation.](https://doctrine-orm.readthedocs.org/en/latest/cookbook/custom-mapping-types.html)

### <a name="custom-functions"></a> Custom Functions

These are classes that extend the functionality of Doctrine's DQL language. More information on what functions are available [visit the repository.](https://github.com/beberlei/DoctrineExtensions) 

To use the extensions in this sample you must install the extensions package:

`require laravel-doctrine/extensions`

and follow the [installation instructions.](http://www.laraveldoctrine.org/docs/1.0/extensions/installation)
 
To add a function simply add it to the correct list using this format:

`'FUNCTION_NAME' => 'Path\To\Class'`

```php
/*
|--------------------------------------------------------------------------
| DQL custom datetime functions
|--------------------------------------------------------------------------
*/
'custom_datetime_functions' => [],
/*
|--------------------------------------------------------------------------
| DQL custom numeric functions
|--------------------------------------------------------------------------
*/
'custom_numeric_functions'  => [],
/*
|--------------------------------------------------------------------------
| DQL custom string functions
|--------------------------------------------------------------------------
*/
'custom_string_functions'   => [],
```

### <a name="logger"></a> Logger

Enable logging of Laravel Doctrine and Doctrine by using the logger functionality.

Available loggers:

* `LaravelDoctrine\ORM\Loggers\LaravelDebugbarLogger`
* `LaravelDoctrine\ORM\Loggers\ClockworkLogger`
* `LaravelDoctrine\ORM\Loggers\FileLogger`

` 'logger' => env('DOCTRINE_LOGGER', false),`

### <a name="cache"></a> Cache

Cache providers available:

* `apc`
* `array`
* `file`
* `memcached`
* `redis`

Properties:

* **default** - The default cache provider to use.
* **namespace** - 
* **second_level** - Should second_level cache be used?

### Gedmo

This is an option for use with **Extensions**

To use this option you must first install the extensions package:

`require laravel-doctrine/extensions`

and follow the [installation instructions.](http://www.laraveldoctrine.org/docs/1.0/extensions/installation)