# Prepr API Wrapper

This package is a wrapper for the <a href="https://prepr.io">Prepr</a> API.

#### Installation

You can install the package via composer:

```bash
composer require graphlr/prepr-api
```

#### Config variables

```text
$apiRequest = new Prepr('token','userId','baseUrl');
```

#### Override variables

The authorization can also be set for one specific request `->url('url')->authorization('token')`.

#### Examples

```php
use Graphlr\Prepr\Prepr;
```

##### Get All

```php
$apiRequest
    ->path('tags')
    ->query([
        'fields' => 'example'
    ])
    ->get();

if($apiRequest->getStatusCode() == 200) {
    print_r($apiRequest->getResponse());
}
```

##### Get Single

```php
$apiRequest
    ->path('tags/{id}',[
        'id' => 1
    ]),
    ->query([
        'fields' => 'example'
    ])
    ->get();

if($apiRequest->getStatusCode() == 200) {
    print_r($apiRequest->getResponse());
}
```

##### Post

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

##### Put

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

##### Delete

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

##### A/B testing custom userId

```php
$apiRequest
    ->path('tags/{id}',[
        'id' => 1
    ]),
    ->query([
        'fields' => 'example'
    ])
    ->userId(
        'testUser'
    )
    ->get();

if($apiRequest->getStatusCode() == 200) {
    print_r($apiRequest->getResponse());
}
```

##### Multipart/Chunk upload

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

##### Autopaging

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


#### Debug

For debug you can use `getRawResponse()`


#### Documentation

<a href="https://developers.prepr.io/docs">For all the details and full documentation check out the Prepr Developer docs</a>
