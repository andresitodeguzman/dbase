# DBase
JSON flat-file database for small PHP projects.


## Getting Started
Download the DBase files. Require the DBase.php file:

```php
require("DBase.php");
```

Now it is time to create and initialize a new database. This is used even if the database exists.

```php
$pie = new DBase("pie");
```

You can also use relative paths to direct DBase to where the DBase.php and database folder is:

```php
require("../DBase.php");
$pie = new DBase("pie", "../db");
```

## DBase Manager
A better way to manage your DBase database is now on its way. Head over to http://github.com/andresitodeguzman/dbase-manager to get a sneak peek to the new DBase Manager. 


## Complete List of Functions
Database functions

- DBase

new DBase(string $name [, string $path])
Initializes the database with a name and database directory

```php
$pie = new DBase("pie", "../db");
```

- rm

rm()
Deletes the database.

```php
echo $pie -> rm();
// Database 'pie' successfully deleted.
// Adding data
```

- rw

rw(array $data)
Rewrites the database with the provided data.

```php
$users = array(array("first" => "Bob", "last" => "Smith"), array("first" => "John", "last" => "Doe"));
$pie -> rw($users);
```

- add

add(array $data)
Appends data to the database.

```php
$pie -> add(array("first" => "Jane", "last" => "Doe"));
$pie -> add(array("first" => "Jim", "last" => "Wales"));
```

# Selecting data

- get

get(string $col, string $key, string $val)
Finds the first row in which the inputted key's value and in inputted value match, and returns the value of the inputted column name.

```php
echo $pie -> get("first","last","Doe"); // John
```

- select

select(array $cols)
Gets a set of columns or all columns for all rows. An empty array selects all columns.

```php
print_r($pie -> select(array("first")));
print_r($pie -> select(array()));
```

- where

where(array $ret, string $key, string $val)
Selects the inputted columns, but only from the rows in which the value of the key matches the inputted value.

```
print_r($pie -> where(array("first"), "last", "Doe"));
print_r($pie -> where(array(), "last", "Doe"));
```

- in

in(array $cols, string $key, array $val)
Selects columns from rows in which the value of the key is a value in the provided array. 

```php
print_r($pie -> in(array("first"), "last", array("Wales", "Doe")));
```

- like

like(array $cols, string $key, string $regex)
Returns columns from rows where the key's value matches the given regular expression.

```php
print_r($pie -> like(array("first"), "last", "/.*s.*/i"));
```

# Multiple databases

Addititonal database data for examples:

```php
$bacon = new DBase("bacon", "../db");
$cheese = new DBase("cheese","../db");
print_r($bacon -> select());
print_r($cheese -> select());
```
- union

union(array $cols, DBase $second)
Merges columns from two databases without duplicates

```php
print_r($pie -> union(array(), $bacon));
```

- join

join(string $method, array $cols, DBase $second, array $match)
Selects columns from rows in both databases based on a key/value pair matching columns in either database. $method can be one of:

"inner": only show rows matched between the two databases
"left": show all rows from acting database, but only matching rows from included database
"right": show all rows from included database, but only matching rows from acting database
"full": show all rows, regardless of matching

```php
print_r($pie -> join("inner", array(), $cheese, array("first" => "name")));
print_r($pie -> join("left", array(), $cheese, array("first" => "name")));
print_r($pie -> join("right", array(), $cheese, array("first" => "name")));
print_r($pie -> join("full", array(), $cheese, array("first" => "name")));
```

Miscellaneous (but useful) functions

- exists

exists(string $key, string $val)
Returns true if there exists the provided key-value pair.

```php
if ($pie -> exists("last", "Smith")) {
echo "Exists";
} else {
echo "Does not exist";
}

if ($pie -> exists("last", "Brown")) {
echo "Exists";
} else {
echo "Does not exist";
}

// Exists
// Does not exist
```

- count

count([string $col])
Counts the number of non-null rows for a certain column. No input counts all columns.

```php
echo $pie -> count("first"); // 4
```

- first & last

first(string $col)
last(string $col)
Selects the first/last value in a column.

```php
echo $pie -> first("last");
echo $pie -> last("first");
// Smith
// Jim
```

## Why Do I Keep this Repository
I don't know why the original author had deleted the original repository of Fllat (original name). He had warned people who opened an issue that he is no longer maintaining the repository but didn't informed anyone about the repository deletion. Since DBase/Fllat lives at the core of all of my PHP-based projects, I will continue to maintain this repository. In the future, I will now name the copyright onto my name if enough edits and improvements would be made in the future. Also, I'll try to add support for PHP 7. This is still meant for personal, internal use and I don't guarantee to give support for any issues. Use at your own risk.