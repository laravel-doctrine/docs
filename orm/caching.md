# Caching

This package supports these caching systems out of the box:

* redis
* memcached
* file
* acp
* array

## Extending or Adding Cache Drivers

Drivers can be replaced or added using `LaravelDoctrine\ORM\Configuration\Cache\CacheManager`. The return should implement `Doctrine\Common\Cache\Cache` or extend `Doctrine\Common\Cache\CacheProvider`

```php
public function boot(CacheManager $cache) {
    $cache->extend('memcache', function(Application $app) {
         $memcache = new \Memcache;
         return new MemcacheCache($memcache);
    });
}
```
