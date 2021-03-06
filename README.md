# Laravel Exchange Rates

<div style="text-align:center">

[![Latest Version on Packagist](https://img.shields.io/packagist/v/ashallendesign/laravel-exchange-rates.svg?style=flat-square)](https://packagist.org/packages/ashallendesign/laravel-exchange-rates)
[![Build Status](https://img.shields.io/travis/ash-jc-allen/laravel-exchange-rates/master.svg?style=flat-square)](https://travis-ci.org/ash-jc-allen/laravel-exchange-rates)
[![Total Downloads](https://img.shields.io/packagist/dt/ashallendesign/laravel-exchange-rates.svg?style=flat-square)](https://packagist.org/packages/ashallendesign/laravel-exchange-rates)
[![PHP from Packagist](https://img.shields.io/packagist/php-v/ashallendesign/laravel-exchange-rates?style=flat-square)](https://img.shields.io/packagist/php-v/ashallendesign/laravel-exchange-rates)
[![GitHub license](https://img.shields.io/github/license/ash-jc-allen/laravel-exchange-rates?style=flat-square)](https://github.com/ash-jc-allen/laravel-exchange-rates/blob/master/LICENSE)

</div>

## Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Usage](#usage)
    - [Methods](#methods)
        - [Available Currencies](#available-currencies)
        - [Exchange Rate](#exchange-rate)
        - [Exchange Rates Between Date Range](#exchange-rates-between-date-range)
        - [Convert Currencies](#convert-currencies)
        - [Convert Currencies Between Date Range](#convert-currencies-between-date-range)
    - [Examples](#examples)
    - [Caching](#caching)
    - [Supported Countries](#supported-countries)
- [Testing](#testing)
- [Security](#security)
- [Contribution](#contribution)
- [License](#license)
    
## Overview

A simple Laravel package used for interacting with the [exchangeratesapi.io](http://exchangeratesapi.io) API. 'Laravel Exchange Rates'
allow you to get the latest or historical exchange rates and convert monetary values between different currencies.

## Installation

You can install the package via Composer:

```bash
composer require ashallendesign/laravel-exchange-rates
```

The package has been developed and tested to work with the following minimum requirements:

- PHP 7.2
- Laravel 5.8

## Usage

### Methods
#### Available Currencies
``` php
$exchangeRates = new ExchangeRate();
$exchangeRates->currencies();
```

#### Exchange Rate
``` php
<?php
$exchangeRates = new ExchangeRate();
$exchangeRates->exchangeRate('GBP', 'EUR');
```
Note: If a Carbon date is passed as the third parameter, the exchange rate for that day will be returned (if valid).
If no date is passed, today's exchange rate will be used.

#### Exchange Rates Between Date Range
``` php
$exchangeRates = new ExchangeRate();
$exchangeRates->exchangeRateBetweenDateRange('GBP', 'EUR', Carbon::now()->subWeek(), Carbon::now());
```

#### Convert Currencies
When passing in the monetary value (first parameter) that is to be converted, it's important that you pass it in the lowest
denomination of that currency. For example, £1 GBP would be passed in as 100 (as £1 = 100 pence).

``` php
$exchangeRates = new ExchangeRate();
$exchangeRates->convert(100, 'GBP', 'EUR', Carbon::now());
```

Note: If a Carbon date is passed as the third parameter, the exchange rate for that day will be returned (if valid).
If no date is passed, today's exchange rate will be used.

#### Convert Currencies Between Date Range
When passing in the monetary value (first parameter) that is to be converted, it's important that you pass it in the lowest
denomination of that currency. For example, £1 GBP would be passed in as 100 (as £1 = 100 pence).

``` php
$exchangeRates = new ExchangeRate();
$exchangeRates->convert(100, 'GBP', 'EUR', Carbon::now()->subWeek(), Carbon::now());
```

### Examples
This example shows how to convert 100 pence (£1) from Great British Pounds to Euros. The current exchange rate will be used (unless a cached rate for this date already exists).
``` php
<?php
    
    namespace App\Http\Controllers;
    
    use AshAllenDesign\LaravelExchangeRates\ExchangeRate;
    
    class TestController extends Controller
    {
        public function index()
        {
            $exchangeRates = new ExchangeRate();
            return $exchangeRates->convert(100, 'GBP', 'EUR', Carbon::now());
        }
    }
```

### Caching
By default, the responses all of the requests to the [exchangeratesapi.io](http://exchangeratesapi.io) API are cached.
This allows for significant performance improvements and reduced bandwidth from your server. 

However, if for any reason you require a fresh result from the API and not a cached result, the ``` ->shouldBustCache() ```
method can be used. The example below shows how to ignore the cached value (if one exists) and make a new API request.

``` php
<?php
    
    namespace App\Http\Controllers;
    
    use AshAllenDesign\LaravelExchangeRates\ExchangeRate;
    
    class TestController extends Controller
    {
        public function index()
        {
            $exchangeRates = new ExchangeRate();
            return $exchangeRates->shouldBustCache()->convert(100, 'GBP', 'EUR', Carbon::now());
        }
    }
```

Note: The caching works by storing exchange rates after fetching them from the API. As an example, if you were to fetch
the exchange rates for 'GBP' to 'EUR' for 20-11-2019 - 27-11-2019, the rates between these dates will be cached as a single
cache item. This cache item will only be retrieved if you attempt to fetch the same exchange rates on with the exact same
currencies and date range.

Therefore, if you were to try and get 'GBP' to 'EUR' for 20-11-2019 - 26-11-2019, a new API request would be made because
the date range is different.

### Supported Countries
Laravel Exchange Rates supports working with the following currencies (sorted in A-Z order):

- ***AUD*** - Australian dollar
- ***BGN*** - Bulgarian lev
- ***BRL*** - Brazilian real
- ***CAD*** - Canadian
- ***CHF*** - Swiss franc
- ***CNY*** - Chinese yuan renminbi
- ***CZK*** - Czech koruna
- ***DKK*** - Danish krone
- ***EUR*** - Euro
- ***GBP*** - Pound sterling
- ***HKD*** - Hong Kong dollar
- ***HRK*** - Croatian kuna
- ***HUF*** - Hungarian forint
- ***IDR*** - Indonesian rupiah
- ***ILS*** - Israeli shekel
- ***INR*** - Indian rupee
- ***ISK*** - Icelandic krone
- ***JPY*** - Japanese yen
- ***KRW*** - South Korean won
- ***MXN*** - Mexican peso
- ***MYR*** - Malaysian ringgit
- ***NOK*** - Norwegian krone
- ***NZD*** - New Zealand dollar
- ***PHP*** - Philippine peso
- ***PLN*** - Polish zloty
- ***RON*** - Romanian leu
- ***RUB*** - Russian rouble
- ***SEK*** - Swedish krona
- ***SGD*** - Singapore dollar
- ***THB*** - Thai baht
- ***TRY*** - Turkish lira
- ***USD*** - US dollar
- ***ZAR*** - South African rand

Note: Please note that the currencies are available because they are exposed in the [exchangeratesapi.io](http://exchangeratesapi.io) API. 

## Testing

``` bash
vendor/bin/phpunit
```

## Security

If you find any security related, please contact me directly at [mail@ashallendesign.co.uk](mailto:mail@ashallendesign.co.uk) to report it.

## Contribution

If you wish to make any changes or improvements to the package, feel free to make a pull request.

Note: A contribution guide will be added soon.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
