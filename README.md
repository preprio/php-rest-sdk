# Getting started with PHP

This package is a wrapper for the REST and GraphQL API.

## Basics
The SDK on [GitHub](https://github.com/preprio/php-sdk)  
Minimal PHP version: `> 5.6.4`   
Requires `GuzzleHttp 7.0.X`

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
    ->path('content_items/{id}', [
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

### Auto paging results

To get all resources for an endpoint, you can use the auto paging feature.

```php
$apiRequest
    ->path('content_items')
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
    ->path('content_items')
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
    ->path('content_items')
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
    ->path('content_items/{id}',[
        'id' => "398402d-dd-asd-ads3343dad"
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
    ->file('/path/to/file.txt')
    ->post();

if($apiRequest->getStatusCode() == 200) {

    print_r($apiRequest->getResponse());
}
```

### Debug Errors

With `$apiRequest->getRawResponse()` you can get the raw response from the Prepr API.
