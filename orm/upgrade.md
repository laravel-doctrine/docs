# Upgrade Guide

## Upgraded Config File

Use one of the commands below with artisan to generate a new `doctrine.php` configuration file from your existing configuration. 

**NOTE: This tool is meant to be used AS A STARTING POINT for converting your configuration. In most cases you will still need to inspect and modify the generated configuration to suite your needs.**

To learn more about flags and how it works see [Migrating Config Files From Other Packages](/docs/{{version}}/orm/config-migrator).

### Upgrading from [atrauzzi/laravel-doctrine](https://github.com/atrauzzi/laravel-doctrine)

`php artisan doctrine:config:convert atrauzzi [--source-file] [--dest-path]`

### Upgrading from [mitchellvanw/laravel-doctrine](https://github.com/mitchellvanw/laravel-doctrine)

`php artisan doctrine:config:convert mitchellvanw [--source-file] [--dest-path]`