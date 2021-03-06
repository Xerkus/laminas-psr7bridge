# Usage

## Converting a PSR-7 ServerRequestInterface to a Laminas\Http\PhpEnvironment\Request

The PSR-7 [ServerRequestInterface](http://www.php-fig.org/psr/psr-7/#321-psrhttpmessageserverrequestinterface)
corresponds to the laminas-http [PhpEnvironment\Request](https://github.com/laminas/laminas-http/blob/master/src/PhpEnvironment/Request.php).

To convert from a PSR-7 instance to a laminas-http instance, use
`Laminas\Psr7Bridge\Psr7ServerRequest::toLaminas()`. This method takes up to two
arguments:

- the `ServerRequestInterface` instance to convert.
- a boolean flag indicating whether or not to do a "shallow" conversion.

*Shallow conversions* omit:

- body parameters ("post" in laminas-http)
- uploaded files
- the body content

It is useful to omit these for purposes of routing, for instance, when you may
not need this more process-intensive data. By default, the `$shallow` flag is
`false`, meaning a full conversion is done.

## Examples

### Full conversion to laminas-http request

```php
use Laminas\Http\PhpEnvironment\Response;
use Laminas\Psr7Bridge\Psr7ServerRequest;

// Assume $controller is a Laminas\Mvc\Controller\AbstractController instance.
$result = $controller->dispatch(
    Psr7ServerRequest::toLaminas($request),
    new Response()
);
```

### Shallow conversion to laminas-http request

```php
use Laminas\Psr7Bridge\Psr7ServerRequest;

// Assume $router is a Laminas\Router\Http\TreeRouteStack instance.
$match = $router->match(Psr7ServerRequest::toLaminas($request, true));
```
