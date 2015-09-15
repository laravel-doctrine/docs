# Caching

This package supports these caching systems out of the box:

* `redis`
* `memcached`
* `file`
* `acp`
* `array`

## Extending or Adding Cache Drivers

Drivers can be replaced or added using `CacheManager`

```php
CacheManager::extend('customCache' function() {
    return 'customCacheProvider'
});
```