# Schema Builder

The Schema Builder is merely a decorator for the DBAL Schema Builder. It makes the process of building migrations similar to Laravel.

### Creating a new table

```
 use LaravelDoctrine\Migrations\Schema\Table;
 use LaravelDoctrine\Migrations\Schema\Builder;

(new Builder($schema))->create('users', function(Table $table) {
    $table->increments('id');
    $table->timestamps();
 });
 ```
 
### Editing an existing table

```
 use LaravelDoctrine\Migrations\Schema\Table;
 use LaravelDoctrine\Migrations\Schema\Builder;

(new Builder($schema))->table('users', function(Table $table) {
    $table->string('name');
 });
 ```

### Available Column Types

Of course, the schema builder contains a variety of column types that you may use when building your tables:

Command  | Description
------------- | -------------
`$table->bigIncrements('id');`  |  Incrementing ID (primary key) using a "UNSIGNED BIG INTEGER" equivalent.
`$table->bigInteger('votes');`  |  BIGINT equivalent for the database.
`$table->binary('data');`  |  BLOB equivalent for the database.
`$table->boolean('confirmed');`  |  BOOLEAN equivalent for the database.
`$table->date('created_at');`  |  DATE equivalent for the database.
`$table->dateTime('created_at');`  |  DATETIME equivalent for the database.
`$table->decimal('amount', 5, 2);`  |  DECIMAL equivalent with a precision and scale.
`$table->float('amount');`  |  FLOAT equivalent for the database.
`$table->increments('id');`  |  Incrementing ID (primary key) using a "UNSIGNED INTEGER" equivalent.
`$table->integer('votes');`  |  INTEGER equivalent for the database.
`$table->json('options');`  |  JSON equivalent for the database.
`$table->nullableTimestamps();`  |  Same as `timestamps()`, except allows NULLs.
`$table->rememberToken();`  |  Adds `remember_token` as VARCHAR(100) NULL.
`$table->smallInteger('votes');`  |  SMALLINT equivalent for the database.
`$table->softDeletes();`  |  Adds `deleted_at` column for soft deletes.
`$table->string('email');`  |  VARCHAR equivalent column.
`$table->string('name', 100);`  |  VARCHAR equivalent with a length.
`$table->text('description');`  |  TEXT equivalent for the database.
`$table->time('sunrise');`  |  TIME equivalent for the database.
`$table->tinyInteger('numbers');`  |  TINYINT equivalent for the database.
`$table->timestamp('added_on');`  |  TIMESTAMP equivalent for the database.
`$table->timestamps();`  |  Adds `created_at` and `updated_at` columns.

### Checking For Table / Column Existence

You may easily check for the existence of a table or column using the `hasTable` and `hasColumn` methods:

```
$builder = (new Builder($schema);

if ($builder->hasTable('users')) {
    //
}

if ($builder->hasColumn('users', 'email')) {
    //
}
```

### Renaming / Dropping Tables

To rename an existing database table, use the `rename` method:

```
(new Builder($schema))->rename($from, $to);
```

To drop an existing table, you may use the `drop` or `dropIfExists` methods:

```
(new Builder($schema))->drop('users');

(new Builder($schema))->dropIfExists('users');
```

### Creating Indexes

You can 