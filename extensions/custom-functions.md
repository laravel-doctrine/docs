# Custom datetime functions

A set of extensions to Doctrine 2 that add support for additional queryfunctions available in MySQL and Oracle.

When `LaravelDoctrine\Extensions\BeberleiExtensionsServiceProvider::class` the following functions will be automatically registerd.

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
        'CarbonDate'       => DoctrineExtensions\Types\CarbonDateType::class,
        'CarbonDateTime'   => DoctrineExtensions\Types\CarbonDateTimeType::class,
        'CarbonDateTimeTz' => DoctrineExtensions\Types\CarbonDateTimeTzType::class,
        'CarbonTime'       => DoctrineExtensions\Types\CarbonTimeType::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom datetime functions
    |--------------------------------------------------------------------------
    */
    'custom_datetime_functions' => [
        'DATE'              => DoctrineExtensions\Query\Mysql\Date::class,
        'DATE_FORMAT'       => DoctrineExtensions\Query\Mysql\DateFormat::class,
        'DATEADD'           => DoctrineExtensions\Query\Mysql\DateAdd::class,
        'DATEDIFF'          => DoctrineExtensions\Query\Mysql\DateDiff::class,
        'DATESUB'           => DoctrineExtensions\Query\Mysql\DateSub::class,
        'DAY'               => DoctrineExtensions\Query\Mysql\Day::class,
        'DAYNAME'           => DoctrineExtensions\Query\Mysql\DayName::class,
        'FROM_UNIXTIME'     => DoctrineExtensions\Query\Mysql\FromUnixtime::class,
        'HOUR'              => DoctrineExtensions\Query\Mysql\Hour::class,
        'LAST_DAY'          => DoctrineExtensions\Query\Mysql\LastDay::class,
        'MINUTE'            => DoctrineExtensions\Query\Mysql\Minute::class,
        'MONTH'             => DoctrineExtensions\Query\Mysql\Month::class,
        'MONTHNAME'         => DoctrineExtensions\Query\Mysql\MonthName::class,
        'SECOND'            => DoctrineExtensions\Query\Mysql\Second::class,
        'STRTODATE'         => DoctrineExtensions\Query\Mysql\StrToDate::class,
        'TIME'              => DoctrineExtensions\Query\Mysql\Time::class,
        'TIMESTAMPADD'      => DoctrineExtensions\Query\Mysql\TimestampAdd::class,
        'TIMESTAMPDIFF'     => DoctrineExtensions\Query\Mysql\TimestampDiff::class,
        'WEEK'              => DoctrineExtensions\Query\Mysql\Week::class,
        'WEEKDAY'           => DoctrineExtensions\Query\Mysql\WeekDay::class,
        'YEAR'              => DoctrineExtensions\Query\Mysql\Year::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom numeric functions
    |--------------------------------------------------------------------------
    */
    'custom_numeric_functions'  => [
        'ACOS'              => DoctrineExtensions\Query\Mysql\Acos::class,
        'ASIN'              => DoctrineExtensions\Query\Mysql\Asin::class,
        'ATAN'              => DoctrineExtensions\Query\Mysql\Atan::class,
        'ATAN2'             => DoctrineExtensions\Query\Mysql\Atan2::class,
        'BINARY'            => DoctrineExtensions\Query\Mysql\Binary::class,
        'CEIL'              => DoctrineExtensions\Query\Mysql\Ceil::class,
        'COS'               => DoctrineExtensions\Query\Mysql\Cos::class,
        'COT'               => DoctrineExtensions\Query\Mysql\Cot::class,
        'COUNTIF'           => DoctrineExtensions\Query\Mysql\Countif::class,
        'CRC32'             => DoctrineExtensions\Query\Mysql\Crc32::class,
        'DEGREES'           => DoctrineExtensions\Query\Mysql\Degrees::class,
        'FLOOR'             => DoctrineExtensions\Query\Mysql\Floor::class,
        'IFELSE'            => DoctrineExtensions\Query\Mysql\ifElse::class,
        'IFNULL'            => DoctrineExtensions\Query\Mysql\ifNull::class,
        'MATCH_AGAINST'     => DoctrineExtensions\Query\Mysql\MatchAgainst::class,
        'NULLIF'            => DoctrineExtensions\Query\Mysql\Nullif::class,
        'PI'                => DoctrineExtensions\Query\Mysql\Pi::class,
        'POWER'             => DoctrineExtensions\Query\Mysql\Power::class,
        'QUARTER'           => DoctrineExtensions\Query\Mysql\Quarter::class,
        'RADIANS'           => DoctrineExtensions\Query\Mysql\Radians::class,
        'RAND'              => DoctrineExtensions\Query\Mysql\Rand::class,
        'ROUND'             => DoctrineExtensions\Query\Mysql\Round::class,
        'SIN'               => DoctrineExtensions\Query\Mysql\Sin::class,
        'STD'               => DoctrineExtensions\Query\Mysql\Std::class,
        'TAN'               => DoctrineExtensions\Query\Mysql\Tan::class,
        'UUID_SHORT'        => DoctrineExtensions\Query\Mysql\UuidShort::class
    ],
    /*
    |--------------------------------------------------------------------------
    | Doctrine custom string functions
    |--------------------------------------------------------------------------
    */
    'custom_string_functions'   => [
        'ASCII'             => DoctrineExtensions\Query\Mysql\Ascii::class,
        'CHAR_LENGTH'       => DoctrineExtensions\Query\Mysql\CharLength::class,
        'CONCAT_WS'         => DoctrineExtensions\Query\Mysql\ConcatWs::class,
        'FIELD'             => DoctrineExtensions\Query\Mysql\Field::class,
        'FIND_IN_SET'       => DoctrineExtensions\Query\Mysql\FindInSet::class,
        'GROUP_CONCAT'      => DoctrineExtensions\Query\Mysql\GroupConcat::class,
        'MD5'               => DoctrineExtensions\Query\Mysql\Md5::class,
        'REGEXP'            => DoctrineExtensions\Query\Mysql\Regexp::class,
        'REPLACE'           => DoctrineExtensions\Query\Mysql\Replace::class,
        'SHA1'              => DoctrineExtensions\Query\Mysql\Sha1::class,
        'SHA2'              => DoctrineExtensions\Query\Mysql\Sha2::class,
        'SOUNDEX'           => DoctrineExtensions\Query\Mysql\Soundex::class,
        'SUBSTRING_INDEX'   => DoctrineExtensions\Query\Mysql\SubstringIndex::class
    ],

];
