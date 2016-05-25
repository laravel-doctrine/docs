# Standalone installation

The fluent driver can be used in any project using Doctrine 2. Simply configure your `EntityManager` with the 
`FluentDriver` as the metadata driver:

```
composer require laravel-doctrine/fluent
```

```php
$fluent = new FluentDriver([
	MyApp\Mappings\ScientistMapping::class,
	MyApp\Mappings\TheoryMapping::class,
	MyApp\Mappings\UniversityMapping::class,
]);

$config = new Doctrine\ORM\Configuration();
$config->setMetadataDriverImpl($fluent);

$entityManager = EntityManager::create($db, $config);
```
