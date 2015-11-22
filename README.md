# mufuphlex raqualizer


[![Build Status](https://travis-ci.org/Mufuphlex/raqualizer.svg?branch=master)](https://travis-ci.org/Mufuphlex/raqualizer)
[![Latest Stable Version](https://poser.pugx.org/mufuphlex/raqualizer/v/stable)](https://packagist.org/packages/mufuphlex/raqualizer)
[![License](https://poser.pugx.org/mufuphlex/raqualizer/license)](https://packagist.org/packages/mufuphlex/raqualizer)


Tool for adjusting values by given etalon and calculated ratios

`Raqualizer` is useful when you have some data from the different sources, and you need to correct one data set in accordance to another one, but want to keep the new values in the old proportion.

## Example
Let's imagine you have two independent sources of statistics containing data about traffic, say _clicks_ and _price_.

The second source has less detailed statistics, but assumed to be the key source of the _price_ value.

The first source is more detailed (e.g., it contains detailed data about _clicks_ and _price_ by traffic types), but the _price_ value there is not the same like in the second source.

`Raqualizer` will help you to edit the first data source values in accordance to the second one:

```php
<?php
use Mufuphlex\Raqualizer;

require_once 'vendor/autoload.php';

// Init etalon values assumed to be correct
$dataSource2Price = 200;
$dataSource2Clicks = 60;

// Init detailed values
// Note the difference between 200 != (150 + 60)
$dataSource1Traffic1Price = 150;
$dataSource1Traffic2Price = 60;
// Note the difference between 60 != (45 + 20)
$dataSource1Traffic1Clicks = 45;
$dataSource1Traffic2Clicks = 20;

// Instantiate Raqualizer values to be corrected
$valueSource1Traffic1Price = new Raqualizer\EqualizableValue($dataSource1Traffic1Price);
$valueSource1Traffic2Price = new Raqualizer\EqualizableValue($dataSource1Traffic2Price);

// Combine Raqualizer values to a Raqualizer Values Set
$priceSet = new Raqualizer\EqualizableValueSet();
$priceSet
    ->addValue($valueSource1Traffic1Price)
    ->addValue($valueSource1Traffic2Price);

// Instantiate Raqualizer processor
$raqualizer = new Raqualizer\Raqualizer();

// Instantiate Raqualizer etalon value
$etalonPrice = new Raqualizer\EtalonValue($dataSource2Price);

// Process values
$correctedPriceValues = $raqualizer->processSet($priceSet, $etalonPrice);

// Ensure that the original values were corrected as expected: 143 + 57 = 200
print_r($correctedPriceValues);
// Note that the values are float due to not round ratios, so in case of need you can round them later
/*
Array
(
    [0] => 142.85714285714  // ~= 143
    [1] => 57.142857142857  // ~= 57
)
*/

// And now we need to edit the clicks data with the same ratio which was applied to price
$valueSource1Traffic1Clicks = new Raqualizer\EqualizableValue($dataSource1Traffic1Clicks);
$valueSource1Traffic2Clicks = new Raqualizer\EqualizableValue($dataSource1Traffic2Clicks);

$clicksSet = new Raqualizer\EqualizableValueSet();
$clicksSet
    ->addValue($valueSource1Traffic1Clicks)
    ->addValue($valueSource1Traffic2Clicks);

$clicksSet->dependsOn($priceSet);
$etalonClicks = new Raqualizer\EtalonValue($dataSource2Clicks);
$correctedClicksValues = $raqualizer->processSet($clicksSet, $etalonClicks);

// Ensure that the original clicks values were corrected as expected as well: 43 + 7 = 60
print_r($correctedClicksValues);
/*
Array
(
    [0] => 42.857142857143  // ~= 43
    [1] => 17.142857142857  // ~= 17
)
*/
```
