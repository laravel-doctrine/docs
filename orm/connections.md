# Connections

Out of the box the database connections configured in `database.php` config are supported (`mysql`, `sqlite`, `pqsql`, `sqlsrv`, `oci8`). 
By simply changing the `database.default` setting you swap the database connection for Doctrine as well.
The additional settings per connection are applied by default.

## Extending or Adding Connections Drivers

You can replace existing connection drivers or add custom drivers using the `ConnectionManager` 

```php
ConnectionManager::extend('myDriver', function() {
    return [
        'driver' => 'driver',
        'host'   => ...
    ];
});
```

You can find the available connection parameters inside the Doctrine documentation: http://doctrine-dbal.readthedocs.org/en/latest/reference/configuration.html