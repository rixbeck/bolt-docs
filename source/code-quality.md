Kód minőség
===========

Ahogy egy projekt növekszik úgy válik egyre nehezebbé megőrizni a kód minőségét.
Minden fejlesztőnek megvan a maga programozási stílusa, hogy hogyan szereti
szervezni a programkódot. Azzal együtt, hogy a fejlesztőknek kell adni bizonyos
szabadságot ezen a téren, mégis szükség van rá, hogy a fejlesztés menetét bizonyos
keretek között tartsuk.

Függőségek kezelése
-------------------
A Github alapértelmezett csomagkezelő eszköze a [Composer](http://getcomposer.org).


Dependency Management
---------------------
The de-facto dependency management tool for Github projects is
[Composer](http://getcomposer.org) We're not trying to re-invent the wheel, so
when possible, we use established and proven libraries to support our
development.

Code Standard
-------------
We aim to comply with [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md).

Unit tests
----------
We use [PHPUnit](https://github.com/sebastianbergmann/phpunit) for unit testing

Continuous Integration and Continuous Inspection
------------------------------------------------
For automatically running our unit tests and checking against various PHP
versions, we use [Travis CI](https://travis-ci.org). The configuration is
located in the
[.travis.yml](https://github.com/bolt/bolt/blob/master/.travis.yml) file. The
code standard, as well as some other helpful tools to get metrics about the
codebase are run by [Scrutinizer CI](https://scrutinizer-ci.com). The
configuration is located in
[.scrutinizer.yml](https://github.com/bolt/bolt/blob/master/.scrutinizer.yml)