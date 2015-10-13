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

The Schema Builder can create three types of indexes. To create a primary key, you can simply use any one of the 
auto-incrementing methods (`bigIncrements` `increments` `smallIncrements`) and it will automatically
create an auto-incrementing unsigned integer column of the specified size and set it as the primary key. 

```
$table->bigIncrements('id');
```

To specify a primary key which does not auto-increment, simply call the `primary` method. For example, consider a weak
entity that represents one of several images of a product and is identified uniquely by the product_id foreign key and the
image ordinal:

```
$table->primary(['product_id', 'position']);
```

To create an index with a unique constraint, call the `unique`
method:

```
$table->unique('email');
```

You can pass an array of column names to any of these methods:

```
$table->index(['inventory_id', 'image_number']);
```

#### Available Index Types

Command  | Description
------------- | -------------
`$table->primary('id');  `  |  Add a primary key.
`$table->primary(['last', 'first', 'zip']);  `  |  Add a composite keys.
`$table->unique('email');  `  |  Add a unique (candidate) key.
`$table->index('country');  ` |  Add a basic index. 
 