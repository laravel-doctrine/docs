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

```
$builder->manyToOne(User::class, 'createdBy')->blameable()->onCreate();
$builder->manyToOne(User::class, 'updatedBy')->blameable()->onUpdate();
$builder->manyToOne(User::class, 'changedBy')->blameable()->onChange(['title', 'body']);
$builder->manyToOne(User::class, 'changedBy')->blameable()->onChange('status', 'Published');

$builder->string('createdBy')->blameable()->onCreate();
$builder->string('updatedBy')->blameable()->onUpdate();
$builder->string('changedBy')->blameable()->onChange(['title', 'body']);
$builder->string('changedBy')->blameable()->onChange('status', 'Published');
```

<a name="iptraceable"></a>
### IpTraceable

```
$builder->string('createdFromIp')->ipTraceable()->onCreate();
$builder->string('updateFromIp')->ipTraceable()->onUpdate();
$builder->string('changedFromIp')->ipTraceable()->onChange(['title', 'body']);
$builder->string('changedFromIp')->ipTraceable()->onChange('status', 'Published');
```

<a name="loggable"></a>
### Loggable

```
$builder->loggable();
$builder->loggable(UserLog::class);

$builder->string('title')->versioned();
$builder->manyToOne(User::class)->versioned();
$builder->OneToMany(User::class)->versioned();
```

<a name="sluggable"></a>
### Sluggable

```
$builder->string('slug')->sluggable('title');
$builder->string('slug')->sluggable(['title', 'code']);

$builder->string('slug')->sluggable('title')
                        ->style('default')
                        ->dateFormat('Y-m-d H:i')
                        ->updatable(true)
                        ->unique(true)
                        ->uniqueBase('base')
                        ->separator('-')
                        ->prefix('prefix-')
                        ->suffix('-suffix');
```

<a name="softdeletes"></a>
### SoftDeletes

```
$builder->softDelete();
$builder->softDelete('deletedAt')->timeAware();
$builder->dateTime('deletedAt')->nullable()->softDelete();
```

<a name="sortable"></a>
### Sortable

```
$builder->integer('position')->sortablePosition();
$builder->string('group')->sortableGroup();
$builder->oneToMany(Category::class)->sortableGroup();
$builder->manyToMany(Category::class)->sortableGroup();
```

<a name="timestamps"></a>
### Timestamps

```
$builder->timestamps(); // Creates both createdAt and updatedAt
$builder->dateTime('createdAt')->timestampable()->onCreate();
$builder->dateTime('updatedAt')->timestampable()->onUpdate();
$builder->dateTime('changedAt')->timestampable()->onChange(['title', 'body']);
$builder->dateTime('changedAt')->timestampable()->onChange('status', 'Published');
```

<a name="translatable"></a>
### Translatable

```
$builder->string('title')->translatable();
$builder->string('locale')->locale();
$builder->translationClass(PageTranslation::class);
```

<a name="tree"></a>
### Tree

```
$builder->nestedSet(); // Creates a default left, right, level and root
$builder->nestedSet()->left('lft')->right('rgt');

$builder->integer('left')->treeLeft();
$builder->integer('right')->treeRight();
$builder->integer('level')->treeLevel();
$builder->integer('root')->treeRoot();
```

<a name="uploadable"></a>
### Uploadable

```
$builder->uploadable();
$builder->uploadable()
        ->allowOverwrite()
        ->appendNumber()
        ->path('somePath')
        ->pathMethod('getPath')
        ->callback('afterFileUpload')
        ->alphanumericFilename()
        ->sha1Filename()
        ->customFilename(FileNameGenerator::class)
        ->maxSize(10000)
        ->allow(['png', 'jgp'])
        ->disallow(['exe']);
        
$builder->string('filePath')->asFilePath();        
$builder->string('fileName')->asFileName();     
$builder->string('fileSize')->asFileSize();     
$builder->string('mineType')->asFileMimeType();     
```
