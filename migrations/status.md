# Migration Status

```
$ php artisan doctrine:migrations:status [--show-versions] [--connection]
```

The command allows to View the status of a set of migrations:

```
 == Configuration
 
    >> Database Driver:         pdo_mysql
    >> Database Name:           testdb
    >> Configuration Source:    ../database/migrations
    >> Version Table Name:      migrations
    >> Migrations Namespace:    Database\Migrations
    >> Current Version:         2015-09-13 14:12:14 (20150913141214)
    >> Latest Version:          2015-09-13 14:12:14 (20150913141214)
    >> Executed Migrations:     0
    >> Available Migrations:    1
    >> New Migrations:          1
 
 == Migration Versions
 
    >> 2015-09-13 14:12:14 (20150913141214)      not migrated
```
