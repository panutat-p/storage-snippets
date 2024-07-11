# Data Types

## CHAR

* Fixed length. Space is reserved to accommodate the specified number of characters
* If the stored string is shorter than the specified length, it will be padded with spaces to fill the allocated space.
* The fixed length makes it easier for the database engine to calculate row offsets, faster for certain operations


## VARCHAR

* Generally faster than `TEXT`
* Data is stored inline with the rest of the table's columns

## TEXT

* Slower performance, additional disk reads
* Stored off the table with the table just storing a pointer to the location of the actual data
* Cannot have a default value
* TINYTEXT: Maximum of 255 characters ~ 1B
* TEXT: Maximum of 65,535 characters ~ 64KB
* MEDIUMTEXT: Maximum of 16,777,215 characters ~ 16MB
* LONGTEXT: Maximum of 4,294,967,295 characters ~ 4GB

## JSON

* Stored off the table with the table just storing a pointer to the location of the actual data
* MySQL validates the JSON data on insertion


## Time

1. `DATE`: stores the date in the format `YYYY-MM-DD` without time information. The range is from `1000-01-01` to `9999-12-31`.
2. `DATETIME`: stores the date and time in the format `YYYY-MM-DD HH:MM:SS`. The range is from `1000-01-01 00:00:00` to `9999-12-31 23:59:59`.
3. `TIMESTAMP`: stores the date and time in the format YYYY-MM-DD HH:MM:SS. However, it has a smaller range, from `1970-01-01 00:00:01` UTC to `2038-01-19 03:14:07` UTC.
The `TIMESTAMP` data type has a special property: its value is converted from the current time zone to UTC while storing, and converted back from UTC to the current time zone when retrieved.

## Decimal

* MySQL's DECIMAL type supports a maximum precision of 65 digits
* `DECIMAL(20,2)` max at 999,999,999,999,999,999.99
