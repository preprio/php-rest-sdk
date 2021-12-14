# Getting started with PHP

This package is a wrapper for the <a href="https://prepr.dev">Prepr</a> REST API.

## Basics
The SDK on [GitHub](https://github.com/preprio/php-sdk)  
Minimal PHP version: `> 5.6.4`   
Requires `GuzzleHttp 7.0.X`, `Murmurhash 2.0.X`

## Installation

You can install the SDK as a composer package.

```bash
composer require preprio/php-sdk
```

## Making your first request

Let's start with getting all publications from your Prepr Environment. 

```php
<?php

use Preprio\Prepr;

$apiRequest = new Prepr('{ACCESS_TOKEN}');

$apiRequest
    ->path('publications')
    ->query([
        'fields' => 'items'
    ])
    ->get();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```


To get a single publication, pass the Id to the request.

```php
<?php

use Preprio\Prepr;

$apiRequest = new Prepr('{ACCESS_TOKEN}');

$apiRequest
    ->path('publications/{id}', [
        'id' => '1236f0b1-b26d-4dde-b835-9e4e441a6d09'
    ])
    ->query([
        'fields' => 'items'
    ])
    ->get();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```


## A/B testing with Optimize

To enable A/B testing you can pass a User ID to provide a consistent result.
The A/B testing feature requires the use of the cached CDN API.

To switch off A/B testing, pass NULL to the UserId param.

```php
$apiRequest = new Prepr('{ACCESS_TOKEN}', '{{YourCustomUserId}}');
```

or per request

```php
$apiRequest
    ->path('publications/{id}',[
        'id' => 1
    ]),
    ->query([
        'fields' => 'example'
    ])
    ->customerId(
        '{{YourCustomUserId}}'
    )
    ->get();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```

For more information check the [Optimize documentation](/docs/optimize/v1/introduction).

## Using the CDN

To use Prepr in production we advise you to use the API CDN for a fast response.
Add the API CDN Url to the init of the SDK.

```php
<?php

use Preprio\Prepr;

$apiRequest = new Prepr('{ACCESS_TOKEN}', null, 'https://cdn.prepr.io/');
```

### Auto paging results

To get all resources for an endpoint, you can use the auto paging feature.

```php
$apiRequest
    ->path('publications')
    ->query([
        'limit' => 200 // optional
    ])
    ->autoPaging();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```

### Override the AccessToken in a request

The authorization can also be set for one specific request `->url('url')->authorization('token')`.

### Post

```php
$apiRequest
    ->path('tags')
    ->params([
        'body' => 'Example'
    ])
    ->post();

if($apiRequest->getStatusCode() == 201) {

    print_r($apiRequest->getResponse());
}
```

### Put

```php
$apiRequest
    ->path('tags')
    ->params([
        'body' => 'Example'
    ])
    ->put();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```

### Delete

```php
$apiRequest
    ->path('tags/{id}',[
        'id' => 1
    ])
    ->delete();

if($apiRequest->getStatusCode() == 204) {

    // Deleted.
}
```

### Multipart/Chunk asset upload

```php
$apiRequest
    ->path('assets')
    ->params([
      'body' => 'Example',
    ])
    ->file('/path/to/file.txt') // For laravel storage: storage_path('app/file.ext')
    ->post();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```

### Debug Errors

With `$apiRequest->getRawResponse()` you can get the raw response from the Prepr API.
