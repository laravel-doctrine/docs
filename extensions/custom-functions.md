# Custom datetime functions

A set of extensions to Doctrine 2 that add support for additional queryfunctions available in MySQL and Oracle.

When `LaravelDoctrine\Extensions\BeberleiExtensionsServiceProvider::class` the following functions will be automatically registered.

| DB | Functions |
|--|---------|
| MySQL | `ACOS, ASCII, ASIN, ATAN, ATAN2, BINARY, CEIL, CHAR_LENGTH, CONCAT_WS, COS, COT, COUNTIF, CRC32, DATE, DATE_FORMAT, DATEADD, DATEDIFF, DATESUB, DAY, DAYNAME, DEGREES, FIELD, FIND_IN_SET, FLOOR, FROM_UNIXTIME, GROUP_CONCAT, HOUR, IFELSE, IFNULL, LAST_DAY, MATCH_AGAINST, MD5, MINUTE, MONTH, MONTHNAME, NULLIF, PI, POWER, QUARTER, RADIANS, RAND, REGEXP, REPLACE, ROUND, SECOND, SHA1, SHA2, SIN, SOUNDEX, STD, STRTODATE, SUBSTRING_INDEX, TAN, TIME, TIMESTAMPADD, TIMESTAMPDIFF, UUID_SHORT, WEEK, WEEKDAY, YEAR` |
| Oracle | `DAY, MONTH, NVL, TODATE, TRUNC, YEAR` |
| Sqlite | `DATE, MINUTE, HOUR, DAY, WEEK, WEEKDAY, MONTH, YEAR, STRFTIME*` |

Alternativly you can include the separate classes inside `config/doctrine` config file. Example:

```
<?php

return [

  // rest of config file

  /*
    |--------------------------------------------------------------------------
    | Doctrine custom types
    |--------------------------------------------------------------------------
    */
    'custom_types'              => [
        'carbondate'       => DoctrineExtensions\Types\CarbonDateType::class,
        'carbondatetime'   => DoctrineExtensions\Types\CarbonDateTimeType::class,
        'carbondatetimetz' => DoctrineExtensions\Types\CarbonDateTimeTzType::class,
        'carbontime'       => DoctrineExtensions\Types\CarbonTimeType::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom datetime functions
    |--------------------------------------------------------------------------
    */
    'custom_datetime_functions' => [
        'DATEADD'  => DoctrineExtensions\Query\Mysql\DateAdd::class,
        'DATEDIFF' => DoctrineExtensions\Query\Mysql\DateDiff::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom numeric functions
    |--------------------------------------------------------------------------
    */
    'custom_numeric_functions'  => [
        'ACOS'    => DoctrineExtensions\Query\Mysql\Acos::class,
        'ASIN'    => DoctrineExtensions\Query\Mysql\Asin::class,
        'ATAN'    => DoctrineExtensions\Query\Mysql\Atan::class,
        'ATAN2'   => DoctrineExtensions\Query\Mysql\Atan2::class,
        'COS'     => DoctrineExtensions\Query\Mysql\Cos::class,
        'COT'     => DoctrineExtensions\Query\Mysql\Cot::class,
        'DEGREES' => DoctrineExtensions\Query\Mysql\Degrees::class,
        'RADIANS' => DoctrineExtensions\Query\Mysql\Radians::class,
        'SIN'     => DoctrineExtensions\Query\Mysql\Sin::class,
        'TAN'     => DoctrineExtensions\Query\Mysql\Ta::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom string functions
    |--------------------------------------------------------------------------
    */
    'custom_string_functions'   => [
        'CHAR_LENGTH' => DoctrineExtensions\Query\Mysql\CharLength::class,
        'CONCAT_WS'   => DoctrineExtensions\Query\Mysql\ConcatWs::class,
        'FIELD'       => DoctrineExtensions\Query\Mysql\Field::class,
        'FIND_IN_SET' => DoctrineExtensions\Query\Mysql\FindInSet::class,
        'REPLACE'     => DoctrineExtensions\Query\Mysql\Replace::class,
        'SOUNDEX'     => DoctrineExtensions\Query\Mysql\Soundex::class,
        'STR_TO_DATE' => DoctrineExtensions\Query\Mysql\StrToDate::class
    ],

];
