# Meta Data

### Annotations

This package supports Doctrine annotations meta data and can be enabled inside the config. 

### XML

This package supports Doctrine xml meta data and can be enabled inside the config.
 
### SimplifiedXML
 
This package supports simplified Doctrine xml meta data and can be enabled inside the config. 

### YAML

This package supports Doctrine yml meta data and can be enabled inside the config. 

### SimplifiedYAML
 
This package supports simplified Doctrine yml meta data and can be enabled inside the config. 

### StaticPhp

This package supports static PHP (`static_php`) meta data and can be enabled inside the config. 

### Config

This package supports using config meta data and can be enabled inside the config.

## Extending or Adding Metadata Drivers

Drivers can be replaced or added using `LaravelDoctrine\ORM\Configuration\MetaData\MetaDataManager`. The callback should return an instance of `\Doctrine\Common\Persistence\Mapping\Driver\MappingDriver`

```php
public function boot(MetaDataManager $metadata) {
    $metadata->extend('myDriver', function(Application $app) {
        return XmlDriver();
    });
}
```
