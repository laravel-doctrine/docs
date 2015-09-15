# Meta Data

### Annotations

This package supports Doctrine annotations meta data and can be enabled inside the config. 

### XML

This package supports Doctrine xml meta data and can be enabled inside the config. 

### YAML

This package supports Doctrine yml meta data and can be enabled inside the config. 

### StaticPhp

This package supports static PHP (`static_php`) meta data and can be enabled inside the config. 

### Config

This package supports using config meta data and can be enabled inside the config.

## Extending or Adding Metadata Drivers

Drivers can be replaced or added using `LaravelDoctrine\ORM\Configuration\MetaData\MetaDataManager`. The callback should return an instance of `Doctrine\ORM\Configuration`

```php
public function boot(MetaDataManager $metadata) {
    $metadata->extend('myDriver' function(Application $app) {
        $config = new Configuration;
        $config->setMetadataCacheImpl($cache);
        $driverImpl = $config->newDefaultAnnotationDriver('/path/to/lib/MyProject/Entities');
        $config->setMetadataDriverImpl($driverImpl);
        $config->setQueryCacheImpl($cache);
        $config->setProxyDir('/path/to/myproject/lib/MyProject/Proxies');
        $config->setProxyNamespace('MyProject\Proxies');
        
        return $config;
    });
}
```
