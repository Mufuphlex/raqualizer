# mufuphlex raqualizer


[![Build Status](https://travis-ci.org/Mufuphlex/raqualizer.svg?branch=master)](https://travis-ci.org/Mufuphlex/raqualizer)
[![Latest Stable Version](https://poser.pugx.org/mufuphlex/raqualizer/v/stable)](https://packagist.org/packages/mufuphlex/raqualizer)
[![License](https://poser.pugx.org/mufuphlex/raqualizer/license)](https://packagist.org/packages/mufuphlex/raqualizer)


Tool for adjusting values by given etalon and calculated ratios

`Raqualizer` is useful when you have some data from the different sources, and you need to correct one data set in accordance to another one, but want to keep the new values in the old proportion.

## Example
Let's imagine you have two independent sources of statistics containing data about traffic, say __clicks__ and __price__.
The second source has less detailed statistics, but assumed to be the key source of the __price__ value. The first source is more detailed (e.g., it contains detailed data about __clicks__ and __price__ by traffic types), but the __price__ value there is not the same like in the second source.
`Raqualizer` will help you to edit the first data source values in accordance to the second one.
