# Hebdate

![Version](https://img.shields.io/packagist/v/kfirba/hebdate.svg)
![Status](https://api.travis-ci.org/kfirba/Hebdate.svg)

Hebdate is a simple library that lets you easily convert gregorian dates to jewish dates, and vice versa, using several formats.

## Installation

Install the package via composer:
    
    composer require kfirba/hebdate
    
## Basic Usage

Let's convert some dates:

```php
// Note that we should pass the date as dd/mm/yyyy format:
GregorianDate::toJewish([5, 6, 2016])->convert();
// -> 28 9 5776
GregorianDate::toJewish('05/06/2016')->convert();
// -> 28 9 5776
GregorianDate::toJewish(Carbon::createFromDate(2016, 6, 5))->convert();
// -> 28 9 5776
GregorianDate::toJewish(new DateTime('2016-06-05'))->convert();
// -> 28 9 5776
```

Naturally, we can use it vice versa:

```php
GregorianDate::fromJewish('28 9 5776')->convert();
// -> 05/06/2016
GregorianDate::fromJewish('28 Iyar 5776')->convert();
// -> 05/06/2016
GregorianDate::fromJewish('כח אייר התשעו')->convert();
// -> 05/06/2016
GregorianDate::fromJewish('כח׳ אייר התשע״ו')->convert();
// -> 05/06/2016
```

Actually, behind the scenes we are using two classes which you can choose whichever you want to use, the `GregorianDate` class and the `JewishDate` class.

By calling `GregorianDate::fromJewish()`, you actually new up a `JewishDate` instance. So it's identical to: 

```php
JewishDate::toGregorian('28 9 5776')->convert();
// -> 05/06/2016
```

As you may have guessed, you can also call:

```php
JewishDate::fromGregorian('05/06/2016')->convert();
// -> 28 9 5776
```

## Input

As we have seen before, the classes are accepting different input formats:

* #### `GregorianDate::toJewish(INPUT_TYPE)`:
    1. **Array** - A plain PHP array that represents the date in [dd, mm, yyyy] format. 
    2. **String** - A string of a gregorian date in the format of mm/dd/yyyy. The delimiter may be a slash(/), a dot(.) or a dash(-). For example: 05-06-2016.
    3. [**Carbon Object**](https://github.com/briannesbitt/Carbon) - Any Carbon object.
    4. [**DateTime Object**](http://php.net/manual/en/class.datetime.php) - Any DateTime object.

* #### `JewishDate::fromGregorian(INPUT_TYPE)`:
    1. Has the same options as `GregorianDate::toJewish(INPUT_TYPE)` as it's just an alias.
    
* #### `JewishDate::toGregorian(INPUT_TYPE)`:
    1. **Array** - an array that contains a hebrew date in [dd, mm, yyyy] format.
    2. **Numeric String** - A numeric string in "dd mm yyyy" format (note that the delimiter has to be a space). For example, 28&nbsp;9&nbsp;5776.
    3. **English month numeric string** - A numeric string with english month name. For example, 28&nbsp;Iyar&nbsp;5776.
    4. **Full hebrew date string** - A full hebrew string without any punctuation. For example, כח&nbsp;אייר&nbsp;התשעו.
    5. **Full hebrew date string with punctuation** - A full hebrew string with punctuation. For example, כח׳&nbsp;אייר&nbsp;התשע״ו.

* #### `GregorianDate::fromJewish(INPUT_TYPE)`:
    1. Has the same options as `JewishDate::toGregorian(INPUT_TYPE)` as it's just an alias.

## Output

We can also specify which output format we want for the date. To do so, we just need to chain the `format()` method and supply it with a format.

**Note that the format constants are under `Kfirba\Formats\Format` class.**

#### Jewish Date Formats:

```php
GregorianDate::toJewish('05/06/2016')->format(Format::NUMERIC)->convert();
// The default format -> 28 9 5776
GregorianDate::toJewish('05/06/2016')->format(Format::ENGLISH_MONTH)->convert();
// -> 28 Iyar 5776
GregorianDate::toJewish('05/06/2016')->format(Format::HEBREW_FULL)->convert();
// -> כח אייר התשעו
GregorianDate::toJewish('05/06/2016')->format(Format::PRESENTABLE_HEBREW_FULL)->convert();
// -> כח׳ אייר התשע״ו

// These formats also apply when using the JewishDate::fromGregorian() constructor:

JewishDate::fromGregorian('05/06/2016')->format(Format::NUMERIC)->convert();
// The default format -> 28 9 5776
JewishDate::fromGregorian('05/06/2016')->format(Format::ENGLISH_MONTH)->convert();
// -> 28 Iyar 5776
JewishDate::fromGregorian('05/06/2016')->format(Format::HEBREW_FULL)->convert();
// -> כח אייר התשעו
JewishDate::fromGregorian('05/06/2016')->format(Format::PRESENTABLE_HEBREW_FULL)->convert();
// -> כח׳ אייר התשע״ו
```

#### Gregorian Date Formats:

```php
JewishDate::toGregorian('28 9 5776')->format(Format::NUMERIC)->convert();
// The default format -> 05/06/2016
JewishDate::toGregorian('28 9 5776')->format(Format::CARBON)->convert();
// -> Carbon Object {}
JewishDate::toGregorian('28 9 5776')->format(Format::DATETIME)->convert();
// -> DateTime Object {}

// These formats also apply when suing the GregorianDate::fromJewish() constructor:

GregorianDate::fromJewish('28 9 5776')->format(Format::NUMERIC)->convert();
// The default format -> 05/06/2016
GregorianDate::fromJewish('28 9 5776')->format(Format::CARBON)->convert();
// -> Carbon Object {}
GregorianDate::fromJewish('28 9 5776')->format(Format::DATETIME)->convert();
// -> DateTime Object {}
```

## Extras

The `convert()` method accepts a `$delimiter` argument. When converting to Jewish dates it defaults to a space (" ") and when converting to Gregorian date it defaults to a slash (/).

The library also registers 2 global functions:

1. `utf8_str_split($str = '', $len = 0)` which acts the same as PHP's `str_split` function but works on UTF8 strings.
2. `isJewishLeapYear($year)` which determines whether the given year is a leap year. Note that the year has to be numeric and greater than 5000.

## License
The Hebdate package is open-source project licensed under the MIT license.
