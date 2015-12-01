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
