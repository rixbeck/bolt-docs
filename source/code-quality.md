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
Nem szeretnénk feltalálni a melegvizet tehát amennyiben lehetséges, kipróbált és
megbízható összetevőket igyekszünk használni a fejlesztéshez.

Coding Standard
---------------
Célunk, hogy eleget tegyünk a [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)
javaslatainak.

Unit tesztek
------------
[PHPUnit](https://github.com/sebastianbergmann/phpunit)-t használunk unit teszteléshez.

Folytonos Integráció és Folytonos Felülvizsgálat
------------------------------------------------

Az automatikus tesztek futtatásához, amik különféle PHP verziók alatt ellenőrzik a
programunkat [Travis CI](https://travis-ci.org)-t használunk. A beállítások a
[.travis.yml](https://github.com/bolt/bolt/blob/master/.travis.yml) fájlban találhatók.
A coding standard ellenőrzését a [Scrutinizer CI](https://scrutinizer-ci.com)
végzi, ami további hasznos adatot is szolgáltat a programkódról. Ennek beállításai
a [.scrutinizer.yml](https://github.com/bolt/bolt/blob/master/.scrutinizer.yml) fájlban
találhatók.
