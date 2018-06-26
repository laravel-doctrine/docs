# Introduction

This package contains extensions for Doctrine2 that hook into the facilities of Doctrine and offer new functionality 
or tools to use Doctrine2 more efficiently. This package contains mostly used behaviors which can be easily attached to your event system
of Doctrine2 and handle the records being flushed in the behavioral way

### Behavioral extensions (Gedmo)

* __Blameable__ - updates string or reference fields on create, update and even property change with a string or object (e.g. user).
* __IpTraceable__ - inherited from Timestampable, sets IP address instead of timestamp
* __Loggable__ - helps tracking changes and history of objects, also supports version management.
* __Sluggable__ - urlizes your specified fields into single unique slug
* __SoftDeleteable__ - allows to implicitly remove records
* __Sortable__ - makes any document or entity sortable
* __Timestampable__ - updates date fields on create, update and even property change.
* __Translatable__ - gives you a very handy solution for translating records into different languages. Easy to setup, easier to use.
* __Tree__ - this extension automates the tree handling process and adds some tree specific functions on repository. (closure, nestedset or materialized path)
* __Uploadable__ - provides file upload handling in entity fields

### Query/Type extensions (Beberlei)

A set of extensions to Doctrine 2 that add support for additional queryfunctions available in MySQL and Oracle.

| DB | Functions |
|--|---------|
| MySQL | `ACOS, ASCII, ASIN, ATAN, ATAN2, BINARY, CEIL, CHAR_LENGTH, CONCAT_WS, COS, COT, COUNTIF, CRC32, DATE, DATE_FORMAT, DATEADD, DATEDIFF, DATESUB, DAY, DAYNAME, DEGREES, FIELD, FIND_IN_SET, FLOOR, FROM_UNIXTIME, GROUP_CONCAT, HOUR, IFELSE, IFNULL, LAST_DAY, MATCH_AGAINST, MD5, MINUTE, MONTH, MONTHNAME, NULLIF, PI, POWER, QUARTER, RADIANS, RAND, REGEXP, REPLACE, ROUND, SECOND, SHA1, SHA2, SIN, SOUNDEX, STD, STRTODATE, SUBSTRING_INDEX, TAN, TIME, TIMESTAMPADD, TIMESTAMPDIFF, UUID_SHORT, WEEK, WEEKDAY, YEAR` |
| Oracle | `DAY, MONTH, NVL, TODATE, TRUNC, YEAR` |
| Sqlite | `DATE, MINUTE, HOUR, DAY, WEEK, WEEKDAY, MONTH, YEAR, STRFTIME*` |
