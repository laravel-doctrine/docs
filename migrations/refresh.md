# Refresh the database

The migrate:refresh command will first roll back all of your database migrations, and then run the migrate command. This command effectively re-creates your entire database.

```
$ php artisan doctrine:migrations:refresh
```
