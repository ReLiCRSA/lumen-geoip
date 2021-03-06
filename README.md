# GeoIP for Lumen

Determine the geographical location of website visitors based on their IP addresses.

----------

## Installation

- [GeoIP for Lumen on GitHub](https://github.com/ReLiCRSA/lumen-geoip)

To get the latest version of GeoIP simply require it in your `composer.json` file.

~~~
"relicrsa/lumen-geoip": "dev-master"
~~~

You'll then need to run `composer update` to download it and have the autoloader updated.

Once GeoIP is installed you need to register the service provider with the application. Open up `bootstrap/app.php` and find the `Register Service Providers` section.

```php
$app->register(ReLiCRSA\GeoIP\GeoIPServiceProvider::class);
$app->configure('geoip');
```

### Configuration files

Browse to `vendors/relicrsa/lumen-geoip/src/config`

Copy `geoip.php` to your `config` directory

There are two supported lookup methods.

The following are the available options

~~~
maxmind = GeoIP2 Either with Webservices or the Downloaded file
legacy = GeoIP Standard Country database
~~~

The GeoIP Standard Country database can be downloaded here
http://dev.maxmind.com/geoip/geolite

The file needs to go to the storage/app directory once downloaded

### Legacy Note

The only data returned on the legacy lookup would be the isoCode, country name, and continent

```php
array (
    "ip"           => "196.2.33.103",
    "isoCode"      => "ZA",
    "country"      => "South Africa",
    "city"         => null,
    "state"        => null,
    "postal_code"  => null,
    "lat"          => null,
    "lon"          => null,
    "timezone"     => null,
    "continent"    => "AF",
    "default"      => false
);
```

### Update max mind cities database

~~~
$ php artisan geoip:update
~~~

**Database Service**: To use the database version of [MaxMind](http://www.maxmind.com) services download the `GeoLite2-City.mmdb` from [http://dev.maxmind.com/geoip/geoip2/geolite2/](http://dev.maxmind.com/geoip/geoip2/geolite2/) and extract it to `storage/app/geoip.mmdb`. And that's it.

## Usage

Get the location data for a website visitor:

```php
use ReLiCRSA\GeoIP\GeoIPFacade;

$location = GeoIP::getLocation();
```

> When an IP is not given the `$_SERVER["REMOTE_ADDR"]` is used.

Getting the location data for a given IP:

```php
use ReLiCRSA\GeoIP\GeoIPFacade;

$location = GeoIP::getLocation('232.223.11.11');
```

### Example Data

```php
array (
    "ip"           => "196.2.33.103",
    "isoCode"      => "ZA",
    "country"      => "South Africa",
    "city"         => null,
    "state"        => null,
    "postal_code"  => null,
    "lat"          => -29.0,
    "lon"          => 24.0,
    "timezone"     => "Africa/Johannesburg",
    "continent"    => "AF",
    "default"      => false
);
```

#### Default Location

In the case that a location is not found the fallback location will be returned with the `default` parameter set to `true`. To set your own default change it in the configurations `config/geoip.php`