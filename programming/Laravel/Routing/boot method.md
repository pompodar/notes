If you would like a route parameter to always be constrained by a given regular expression, you may use the `pattern` method. You should define these patterns in the `boot` method of your `App\Providers\RouteServiceProvider` class:

```
/** * Define your route model bindings, pattern filters, etc. */public function boot(): void{    Route::pattern('id', '[0-9]+');}
```

Once the pattern has been defined, it is automatically applied to all routes using that parameter name:

```
Route::get('/user/{id}', function (string $id) {    // Only executed if {id} is numeric...});
```


Typically, rate limiters are defined within the `boot` method of your application's `App\Providers\RouteServiceProvider` class. In fact, this class already includes a rate limiter definition that is applied to the routes in your application's `routes/api.php` file:

```
use Illuminate\Cache\RateLimiting\Limit;use Illuminate\Http\Request;use Illuminate\Support\Facades\RateLimiter; /** * Define your route model bindings, pattern filters, and other route configuration. */protected function boot(): void{    RateLimiter::for('api', function (Request $request) {        return Limit::perMinute(60)->by($request->user()?->id ?: $request->ip());    });     // ...
```


https://laravel.com/docs/10.x/routing