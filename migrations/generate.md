# Generate Migration

```
$ php artisan doctrine:migrations:generate [--create=table_name] [--table=table_name]
```

The new migration will be placed in your database/migrations directory. Each migration file name contains a timestamp as version which allows Doctrine to determine the order of the migrations.

The `--table` and `--create` options may also be used to indicate the name of the table and whether the migration will be creating a new table. These options simply pre-fill the generated migration stub file with the specified table:

````
php artisan doctrine:migrations:generate --table=users

php artisan doctrine:migrations:generate --create=users
```

If you would like to specify another output path for the generated migration, you can change the setting in `config/migrations`.

The command generates blank migration class in `database/migrations` with latest version.

```php
<?php
namespace Database\Migrations;

use Doctrine\Migrations\AbstractMigration;
use Doctrine\DBAL\Schema\Schema;

class Version20150915130401 extends AbstractMigration
{
 public function up(Schema $schema): void
 {

 }

 public function down(Schema $schema): void
 {

 }
}
```

### SQL

`Schema` has `addSql()` method to add our own custom SQL queries.

For example:

```php
public function up(Schema $schema): void
{
    $this->addSql('CREATE TABLE addresses (id INT NOT NULL, street VARCHAR(255) NOT NULL, PRIMARY KEY(id)) ENGINE = InnoDB');
}
```

### Schema Builder

Alternatively you can use a Laravel-like Schema Builder. The previous example will now look like this:
 
 ```php
 
 use LaravelDoctrine\Migrations\Schema\Table;
 use LaravelDoctrine\Migrations\Schema\Builder;
 
 public function up(Schema $schema): void
 {
     (new Builder($schema))->create('addresses', function(Table $table) {
        $table->increments('id');
        $table->string('street');
     });
 }
 ```
