# Configuration

`config/migrations.php` holds a couple of configuration settings. 

## Migrations Table

This database table keeps track of all the migrations that have already run for your application. Using this information, we can determine which of the migrations on disk haven't actually been run in the database.

__Default__: migrations

## Migration Directory

This directory is where all migrations will be stored

__Default__: database/migrations

## Migration Namespace

This namespace will be used on all migrations

__Default__: Database\\Migrations

## Migration Schema

Tables which are filtered by Regular Expression. You optionally exclude or limit to certain tables. The default will allow all tables.

__Default__: '/^(?).*$/'


## Migration Version Column Length

The length for the version column in the migrations table. By default migrations have are named like `Version20150914223731`. If you are customizing this name then you will need to expand the column length.

__Default__: '14'
