# Execute migration

```
$ php artisan doctrine:migrations:execute [version] [--up] [--down]
```

Executes a single migration version up or down manually.

```
$ php artisan doctrine:migrations:execute 20150914223731 --up

    > Migrated: 20150914223731
```


```
$ php artisan doctrine:migrations:execute 20150914223731 --down

    > Rolled back: 20150914223731
```
