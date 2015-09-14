# Add or Delete versions

```php
$ php artisan doctrine:migrations:version [version] [--add] [--delete]
```

Sometimes you may need to manually change something in the database table which manages the versions for some migrations. For this you can use the version task. You can easily add a version like this:

```
$ php artisan doctrine:migrations:version YYYYMMDDHHMMSS --add
```

Or you can delete that version:

```
$ php artisan doctrine:migrations:version YYYYMMDDHHMMSS --delete
```

The command does not execute any migrations code, it simply adds the specified version to the database.
