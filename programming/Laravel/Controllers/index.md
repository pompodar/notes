## [Introduction](https://laravel.com/docs/10.x/controllers#introduction)

Instead of defining all of your request handling logic as closures in your route files, you may wish to organize this behavior using "controller" classes. Controllers can group related request handling logic into a single class. For example, a `UserController` class might handle all incoming requests related to users, including showing, creating, updating, and deleting users. By default, controllers are stored in the `app/Http/Controllers` directory.

## [Writing Controllers](https://laravel.com/docs/10.x/controllers#writing-controllers)

### [Basic Controllers](https://laravel.com/docs/10.x/controllers#basic-controllers)

To quickly generate a new controller, you may run the `make:controller` Artisan command. By default, all of the controllers for your application are stored in the `app/Http/Controllers` directory:

```
php artisan make:controller UserController
```

Let's take a look at an example of a basic controller. A controller may have any number of public methods which will respond to incoming HTTP requests:

```
<?php namespace App\Http\Controllers; use App\Models\User;use Illuminate\View\View; class UserController extends Controller{    /**     * Show the profile for a given user.     */    public function show(string $id): View    {        return view('user.profile', [            'user' => User::findOrFail($id)        ]);    }}
```

Once you have written a controller class and method, you may define a route to the controller method like so:

```
use App\Http\Controllers\UserController; Route::get('/user/{id}', [UserController::class, 'show']);
```

When an incoming request matches the specified route URI, the `show` method on the `App\Http\Controllers\UserController` class will be invoked and the route parameters will be passed to the method.

> ![](https://laravel.com/img/callouts/lightbulb.min.svg)
> 
> Controllers are not **required** to extend a base class. However, you will not have access to convenient features such as the `middleware` and `authorize` methods.

### [Single Action Controllers](https://laravel.com/docs/10.x/controllers#single-action-controllers)

If a controller action is particularly complex, you might find it convenient to dedicate an entire controller class to that single action. To accomplish this, you may define a single `__invoke` method within the controller:

```
<?php namespace App\Http\Controllers; class ProvisionServer extends Controller{    /**     * Provision a new web server.     */    public function __invoke()    {        // ...    }}
```

When registering routes for single action controllers, you do not need to specify a controller method. Instead, you may simply pass the name of the controller to the router:

```
use App\Http\Controllers\ProvisionServer; Route::post('/server', ProvisionServer::class);
```

You may generate an invokable controller by using the `--invokable` option of the `make:controller` Artisan command:

```
php artisan make:controller ProvisionServer --invokable
```

> ![](https://laravel.com/img/callouts/lightbulb.min.svg)
> 
> Controller stubs may be customized using [stub publishing](https://laravel.com/docs/10.x/artisan#stub-customization).

## [Controller Middleware](https://laravel.com/docs/10.x/controllers#controller-middleware)

[Middleware](https://laravel.com/docs/10.x/middleware) may be assigned to the controller's routes in your route files:

```
Route::get('profile', [UserController::class, 'show'])->middleware('auth');
```

Or, you may find it convenient to specify middleware within your controller's constructor. Using the `middleware` method within your controller's constructor, you can assign middleware to the controller's actions:

```
class UserController extends Controller{    /**     * Instantiate a new controller instance.     */    public function __construct()    {        $this->middleware('auth');        $this->middleware('log')->only('index');        $this->middleware('subscribed')->except('store');    }}
```

Controllers also allow you to register middleware using a closure. This provides a convenient way to define an inline middleware for a single controller without defining an entire middleware class:

```
use Closure;use Illuminate\Http\Request; $this->middleware(function (Request $request, Closure $next) {    return $next($request);});
```

## [Resource Controllers](https://laravel.com/docs/10.x/controllers#resource-controllers)

If you think of each Eloquent model in your application as a "resource", it is typical to perform the same sets of actions against each resource in your application. For example, imagine your application contains a `Photo` model and a `Movie` model. It is likely that users can create, read, update, or delete these resources.

Because of this common use case, Laravel resource routing assigns the typical create, read, update, and delete ("CRUD") routes to a controller with a single line of code. To get started, we can use the `make:controller` Artisan command's `--resource` option to quickly create a controller to handle these actions:

```
php artisan make:controller PhotoController --resource
```

This command will generate a controller at `app/Http/Controllers/PhotoController.php`. The controller will contain a method for each of the available resource operations. Next, you may register a resource route that points to the controller:

```
use App\Http\Controllers\PhotoController; Route::resource('photos', PhotoController::class);
```

This single route declaration creates multiple routes to handle a variety of actions on the resource. The generated controller will already have methods stubbed for each of these actions. Remember, you can always get a quick overview of your application's routes by running the `route:list` Artisan command.

You may even register many resource controllers at once by passing an array to the `resources` method:

```
Route::resources([    'photos' => PhotoController::class,    'posts' => PostController::class,]);
```

#### [Actions Handled By Resource Controller](https://laravel.com/docs/10.x/controllers#actions-handled-by-resource-controller)

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/photos`|index|photos.index|
|GET|`/photos/create`|create|photos.create|
|POST|`/photos`|store|photos.store|
|GET|`/photos/{photo}`|show|photos.show|
|GET|`/photos/{photo}/edit`|edit|photos.edit|
|PUT/PATCH|`/photos/{photo}`|update|photos.update|
|DELETE|`/photos/{photo}`|destroy|photos.destroy|

#### [Customizing Missing Model Behavior](https://laravel.com/docs/10.x/controllers#customizing-missing-model-behavior)

Typically, a 404 HTTP response will be generated if an implicitly bound resource model is not found. However, you may customize this behavior by calling the `missing` method when defining your resource route. The `missing` method accepts a closure that will be invoked if an implicitly bound model can not be found for any of the resource's routes:

```
use App\Http\Controllers\PhotoController;use Illuminate\Http\Request;use Illuminate\Support\Facades\Redirect; Route::resource('photos', PhotoController::class)        ->missing(function (Request $request) {            return Redirect::route('photos.index');        });
```

#### [Soft Deleted Models](https://laravel.com/docs/10.x/controllers#soft-deleted-models)

Typically, implicit model binding will not retrieve models that have been [soft deleted](https://laravel.com/docs/10.x/eloquent#soft-deleting), and will instead return a 404 HTTP response. However, you can instruct the framework to allow soft deleted models by invoking the `withTrashed` method when defining your resource route:

```
use App\Http\Controllers\PhotoController; Route::resource('photos', PhotoController::class)->withTrashed();
```

Calling `withTrashed` with no arguments will allow soft deleted models for the `show`, `edit`, and `update` resource routes. You may specify a subset of these routes by passing an array to the `withTrashed` method:

```
Route::resource('photos', PhotoController::class)->withTrashed(['show']);
```

#### [Specifying The Resource Model](https://laravel.com/docs/10.x/controllers#specifying-the-resource-model)

If you are using [route model binding](https://laravel.com/docs/10.x/routing#route-model-binding) and would like the resource controller's methods to type-hint a model instance, you may use the `--model` option when generating the controller:

```
php artisan make:controller PhotoController --model=Photo --resource
```

#### [Generating Form Requests](https://laravel.com/docs/10.x/controllers#generating-form-requests)

You may provide the `--requests` option when generating a resource controller to instruct Artisan to generate [form request classes](https://laravel.com/docs/10.x/validation#form-request-validation) for the controller's storage and update methods:

```
php artisan make:controller PhotoController --model=Photo --resource --requests
```

### [Partial Resource Routes](https://laravel.com/docs/10.x/controllers#restful-partial-resource-routes)

When declaring a resource route, you may specify a subset of actions the controller should handle instead of the full set of default actions:

```
use App\Http\Controllers\PhotoController; Route::resource('photos', PhotoController::class)->only([    'index', 'show']); Route::resource('photos', PhotoController::class)->except([    'create', 'store', 'update', 'destroy']);
```

#### [API Resource Routes](https://laravel.com/docs/10.x/controllers#api-resource-routes)

When declaring resource routes that will be consumed by APIs, you will commonly want to exclude routes that present HTML templates such as `create` and `edit`. For convenience, you may use the `apiResource` method to automatically exclude these two routes:

```
use App\Http\Controllers\PhotoController; Route::apiResource('photos', PhotoController::class);
```

You may register many API resource controllers at once by passing an array to the `apiResources` method:

```
use App\Http\Controllers\PhotoController;use App\Http\Controllers\PostController; Route::apiResources([    'photos' => PhotoController::class,    'posts' => PostController::class,]);
```

To quickly generate an API resource controller that does not include the `create` or `edit` methods, use the `--api` switch when executing the `make:controller` command:

```
php artisan make:controller PhotoController --api
```

### [Nested Resources](https://laravel.com/docs/10.x/controllers#restful-nested-resources)

Sometimes you may need to define routes to a nested resource. For example, a photo resource may have multiple comments that may be attached to the photo. To nest the resource controllers, you may use "dot" notation in your route declaration:

```
use App\Http\Controllers\PhotoCommentController; Route::resource('photos.comments', PhotoCommentController::class);
```

This route will register a nested resource that may be accessed with URIs like the following:

```
/photos/{photo}/comments/{comment}
```

#### [Scoping Nested Resources](https://laravel.com/docs/10.x/controllers#scoping-nested-resources)

Laravel's [implicit model binding](https://laravel.com/docs/10.x/routing#implicit-model-binding-scoping) feature can automatically scope nested bindings such that the resolved child model is confirmed to belong to the parent model. By using the `scoped` method when defining your nested resource, you may enable automatic scoping as well as instruct Laravel which field the child resource should be retrieved by. For more information on how to accomplish this, please see the documentation on [scoping resource routes](https://laravel.com/docs/10.x/controllers#restful-scoping-resource-routes).

#### [Shallow Nesting](https://laravel.com/docs/10.x/controllers#shallow-nesting)

Often, it is not entirely necessary to have both the parent and the child IDs within a URI since the child ID is already a unique identifier. When using unique identifiers such as auto-incrementing primary keys to identify your models in URI segments, you may choose to use "shallow nesting":

```
use App\Http\Controllers\CommentController; Route::resource('photos.comments', CommentController::class)->shallow();
```

This route definition will define the following routes:

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/photos/{photo}/comments`|index|photos.comments.index|
|GET|`/photos/{photo}/comments/create`|create|photos.comments.create|
|POST|`/photos/{photo}/comments`|store|photos.comments.store|
|GET|`/comments/{comment}`|show|comments.show|
|GET|`/comments/{comment}/edit`|edit|comments.edit|
|PUT/PATCH|`/comments/{comment}`|update|comments.update|
|DELETE|`/comments/{comment}`|destroy|comments.destroy|

### [Naming Resource Routes](https://laravel.com/docs/10.x/controllers#restful-naming-resource-routes)

By default, all resource controller actions have a route name; however, you can override these names by passing a `names` array with your desired route names:

```
use App\Http\Controllers\PhotoController; Route::resource('photos', PhotoController::class)->names([    'create' => 'photos.build']);
```

### [Naming Resource Route Parameters](https://laravel.com/docs/10.x/controllers#restful-naming-resource-route-parameters)

By default, `Route::resource` will create the route parameters for your resource routes based on the "singularized" version of the resource name. You can easily override this on a per resource basis using the `parameters` method. The array passed into the `parameters` method should be an associative array of resource names and parameter names:

```
use App\Http\Controllers\AdminUserController; Route::resource('users', AdminUserController::class)->parameters([    'users' => 'admin_user']);
```

The example above generates the following URI for the resource's `show` route:

```
/users/{admin_user}
```

### [Scoping Resource Routes](https://laravel.com/docs/10.x/controllers#restful-scoping-resource-routes)

Laravel's [scoped implicit model binding](https://laravel.com/docs/10.x/routing#implicit-model-binding-scoping) feature can automatically scope nested bindings such that the resolved child model is confirmed to belong to the parent model. By using the `scoped` method when defining your nested resource, you may enable automatic scoping as well as instruct Laravel which field the child resource should be retrieved by:

```
use App\Http\Controllers\PhotoCommentController; Route::resource('photos.comments', PhotoCommentController::class)->scoped([    'comment' => 'slug',]);
```

This route will register a scoped nested resource that may be accessed with URIs like the following:

```
/photos/{photo}/comments/{comment:slug}
```

When using a custom keyed implicit binding as a nested route parameter, Laravel will automatically scope the query to retrieve the nested model by its parent using conventions to guess the relationship name on the parent. In this case, it will be assumed that the `Photo` model has a relationship named `comments` (the plural of the route parameter name) which can be used to retrieve the `Comment` model.

### [Localizing Resource URIs](https://laravel.com/docs/10.x/controllers#restful-localizing-resource-uris)

By default, `Route::resource` will create resource URIs using English verbs and plural rules. If you need to localize the `create` and `edit` action verbs, you may use the `Route::resourceVerbs` method. This may be done at the beginning of the `boot` method within your application's `App\Providers\RouteServiceProvider`:

```
/** * Define your route model bindings, pattern filters, etc. */public function boot(): void{    Route::resourceVerbs([        'create' => 'crear',        'edit' => 'editar',    ]);     // ...}
```

Laravel's pluralizer supports [several different languages which you may configure based on your needs](https://laravel.com/docs/10.x/localization#pluralization-language). Once the verbs and pluralization language have been customized, a resource route registration such as `Route::resource('publicacion', PublicacionController::class)` will produce the following URIs:

```
/publicacion/crear /publicacion/{publicaciones}/editar
```

### [Supplementing Resource Controllers](https://laravel.com/docs/10.x/controllers#restful-supplementing-resource-controllers)

If you need to add additional routes to a resource controller beyond the default set of resource routes, you should define those routes before your call to the `Route::resource` method; otherwise, the routes defined by the `resource` method may unintentionally take precedence over your supplemental routes:

```
use App\Http\Controller\PhotoController; Route::get('/photos/popular', [PhotoController::class, 'popular']);Route::resource('photos', PhotoController::class);
```

> ![](https://laravel.com/img/callouts/lightbulb.min.svg)
> 
> Remember to keep your controllers focused. If you find yourself routinely needing methods outside of the typical set of resource actions, consider splitting your controller into two, smaller controllers.

### [Singleton Resource Controllers](https://laravel.com/docs/10.x/controllers#singleton-resource-controllers)

Sometimes, your application will have resources that may only have a single instance. For example, a user's "profile" can be edited or updated, but a user may not have more than one "profile". Likewise, an image may have a single "thumbnail". These resources are called "singleton resources", meaning one and only one instance of the resource may exist. In these scenarios, you may register a "singleton" resource controller:

```
use App\Http\Controllers\ProfileController;use Illuminate\Support\Facades\Route; Route::singleton('profile', ProfileController::class);
```

The singleton resource definition above will register the following routes. As you can see, "creation" routes are not registered for singleton resources, and the registered routes do not accept an identifier since only one instance of the resource may exist:

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/profile`|show|profile.show|
|GET|`/profile/edit`|edit|profile.edit|
|PUT/PATCH|`/profile`|update|profile.update|

Singleton resources may also be nested within a standard resource:

```
Route::singleton('photos.thumbnail', ThumbnailController::class);
```

In this example, the `photos` resource would receive all of the [standard resource routes](https://laravel.com/docs/10.x/controllers#actions-handled-by-resource-controller); however, the `thumbnail` resource would be a singleton resource with the following routes:

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/photos/{photo}/thumbnail`|show|photos.thumbnail.show|
|GET|`/photos/{photo}/thumbnail/edit`|edit|photos.thumbnail.edit|
|PUT/PATCH|`/photos/{photo}/thumbnail`|update|photos.thumbnail.update|

#### [Creatable Singleton Resources](https://laravel.com/docs/10.x/controllers#creatable-singleton-resources)

Occasionally, you may want to define creation and storage routes for a singleton resource. To accomplish this, you may invoke the `creatable` method when registering the singleton resource route:

```
Route::singleton('photos.thumbnail', ThumbnailController::class)->creatable();
```

In this example, the following routes will be registered. As you can see, a `DELETE` route will also be registered for creatable singleton resources:

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/photos/{photo}/thumbnail/create`|create|photos.thumbnail.create|
|POST|`/photos/{photo}/thumbnail`|store|photos.thumbnail.store|
|GET|`/photos/{photo}/thumbnail`|show|photos.thumbnail.show|
|GET|`/photos/{photo}/thumbnail/edit`|edit|photos.thumbnail.edit|
|PUT/PATCH|`/photos/{photo}/thumbnail`|update|photos.thumbnail.update|
|DELETE|`/photos/{photo}/thumbnail`|destroy|photos.thumbnail.destroy|

If you would like Laravel to register the `DELETE` route for a singleton resource but not register the creation or storage routes, you may utilize the `destroyable` method:

```
Route::singleton(...)->destroyable();
```

#### [API Singleton Resources](https://laravel.com/docs/10.x/controllers#api-singleton-resources)

The `apiSingleton` method may be used to register a singleton resource that will be manipulated via an API, thus rendering the `create` and `edit` routes unnecessary:

```
Route::apiSingleton('profile', ProfileController::class);
```

Of course, API singleton resources may also be `creatable`, which will register `store` and `destroy` routes for the resource:

```
Route::apiSingleton('photos.thumbnail', ProfileController::class)->creatable();
```

## [Dependency Injection & Controllers](https://laravel.com/docs/10.x/controllers#dependency-injection-and-controllers)

#### [Constructor Injection](https://laravel.com/docs/10.x/controllers#constructor-injection)

The Laravel [service container](https://laravel.com/docs/10.x/container) is used to resolve all Laravel controllers. As a result, you are able to type-hint any dependencies your controller may need in its constructor. The declared dependencies will automatically be resolved and injected into the controller instance:

```
<?php namespace App\Http\Controllers; use App\Repositories\UserRepository; class UserController extends Controller{    /**     * Create a new controller instance.     */    public function __construct(        protected UserRepository $users,    ) {}}
```

#### [Method Injection](https://laravel.com/docs/10.x/controllers#method-injection)

In addition to constructor injection, you may also type-hint dependencies on your controller's methods. A common use-case for method injection is injecting the `Illuminate\Http\Request` instance into your controller methods:

```
<?php namespace App\Http\Controllers; use Illuminate\Http\RedirectResponse;use Illuminate\Http\Request; class UserController extends Controller{    /**     * Store a new user.     */    public function store(Request $request): RedirectResponse    {        $name = $request->name;         // Store the user...         return redirect('/users');    }}
```

If your controller method is also expecting input from a route parameter, list your route arguments after your other dependencies. For example, if your route is defined like so:

```
use App\Http\Controllers\UserController; Route::put('/user/{id}', [UserController::class, 'update']);
```

You may still type-hint the `Illuminate\Http\Request` and access your `id` parameter by defining your controller method as follows:

```
<?php namespace App\Http\Controllers; use Illuminate\Http\RedirectResponse;use Illuminate\Http\Request; class UserController extends Controller{    /**     * Update the given user.     */    public function update(Request $request, string $id): RedirectResponse    {        // Update the user...         return redirect('/users');    }}
```

https://laravel.com/docs/10.x/controllers