# Rollback migration

```
$ php artisan doctrine:migrations:rollback [version?]
```

To rollback the latest migration "operation", you may use the rollback command. 
Note that this rolls back the previous "version" or a specific version

```
$ php artisan doctrine:migrations:rollback

    > Rolled back: 20150914223731
```

```
$ php artisan doctrine:migrations:rollback 20150914223731

    > Rolled back: 20150914223731
```
