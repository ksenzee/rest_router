REST Router
===========

Module that allows creating multiple REST endpoints defined in code. No UI is
included in this module. Key features:

  - lightweight
  - supports versions of API with plugins
  - authentication plugins
  - routing to objects

### Using router

To create a new REST endpoint, your module needs to implement
`hook_rest_endpoints`. Hook documentation is available in
`rest_router.api.php`.

##### Custom responses

Object or function callbacks called by rest_router module can return responses
in different data formats. Scalar responses are always converted to arrays.

```php
    function my_callback() {
        return "value";
    }
```

Will become in response data

```php
    array("value")
```

If a response other than `200 OK` is required, the function can return a defined
response object:

- `new RestRouterResponse($code, $data = NULL)`
- `RestRouterErrorResponse($code, $message, $data = NULL)`
- `RestRouterRedirectResponse($path, $code)`

Each response has a helper method that can be used from the parent class.

**Examples**

```php
    public function myMethod() {
        // Custom response by initializing class
        return new RestRouterResponse(204);

        // Response via internal helper
        return $this->errorResponse(400, "Missing param [1]");

        // Redirect response example
        return $this->redirectResponse('method/2');
    }
```

### Ideas

- Allow API version definition to override the 'auth', 'request', and 'response'
settings.

### Tests

Unit tests are for classes. To run them:

    drush test-run "Rest Router Unit"

Web test cases can be run with

    drush test-run "Rest Router"
