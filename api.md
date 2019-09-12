# API
* [Introduction](#introduction)
* [Configuration](#configuration)
* [Defining API Route](#defining-api-route)
* [Accessing API Route](#accessing-api-route)
	* [From JavaScript Application](#from-javascript-application)
* [Scopes](#scopes)
* [Checking Scopes](#checking-scopes)
	* [Via Middleware](#via-middleware)
	* [Via TokenCan Method](#via-token-can-method)

## [Introduction](#introduction)
Larakits comes with API development support. It allows your user to create API tokens for authenticating your API routes. Optionally, you can register token scopes from where your user can determine which action this API token can perform.

If you check `configs/auth.php`  file, you will see `larakits` as custom API authentication guard. This guard allows you to access your API routes without passing API token from your JavaScript  Application. If you are requesting API routes from SDK, make sure you are passing valid API token. 

## [Configuration](#configuration)
By default, API support is disabled for your application. You would not see any link for the API page on `/settings` page. To enable API support, you must set `TRUE` for the `$api` property of `App\Providers\LarakitsServiceProvider` class.

## [Defining API Route](#defining-api-route)
Defining API route is straightforward. If you missed [Laravel docs for API authentication](https://laravel.com/docs/6.0/api-authentication), here an example for you:

```
use Illuminate\Http\Request;

Route::middleware('auth:api')->get('/user', function(Request $request) {
	return $request->user();
});
```

In the example, `/user` route is defined with `auth:api` middleware. The auth middleware will ensure that non-authenticate user can’t access the route. If you want to create route for non-authenticate user, leave the `auth:api` middleware.

## [Accessing API Route](#accessing-api-route)
Once API route is defined, user can access the API route by their API token. They have to pass their API token via `api_token` query string or as a `Bearer` token in the `Authorization` header of the request.

### [From JavaScript Application](#from-javascript-application)
When you are building your application that shares your API between your JavaScript application and SDK, your JavaScript application have to pass `API_TOKEN` for all requests you will send to API routes. 

To reduce the pain, Larakits uses custom `larakits` authentication guard and `Larakits\Http\Middleware\CreateFreshApiToken` middleware. Both will make sure that you do not need to pass any API token while accessing your API route from JavaScript Application. The authentication is automatically handled by Larakits. 

If you are using `axios` as HTTP client, you can call your API route like a normal web route:

```
axios.get('/api/user')
  .then(response => {
    console.log(response)
  })
```

## [Scopes](#scopes)
The scopes is the way of limiting API access. User can grant the ability when creating new API token. To give user that opportunity you have to register the token’s scopes first. Your registered scopes will automatically show up on the API creating modal.

To register scopes, you may use `Larakits::tokensCan` method in the `booted` method of `App\Providers\LarakitsServiceProvide`:

```
Larakits::tokensCan([
   'create-user' => 'Create User',
   'delete-user => 'Delete User'
]);
```

## [Checking Scopes](#checking-scopes)
When a user is authenticated via API, you can perform the ability check for API token in two different ways:

### [Via Middleware](#via-middleware)
```
Route::middleware(['auth:api', 'token-can:create-user'])->post('/user', function () {
    //
});
```

### [Via TokenCan Method](#via-token-can-method)
```
Route::middleware(['auth:api'])->post('/user', function () {
    if(auth()->user()->tokenCan('create-user')) {
        //
    }
});
```