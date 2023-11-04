[Laravel Breeze](https://github.com/laravel/breeze) is a minimal, simple implementation of all of Laravel's [authentication features](https://laravel.com/docs/10.x/authentication), including login, registration, password reset, email verification, and password confirmation. In addition, Breeze includes a simple "profile" page where the user may update their name, email address, and password.

Laravel Breeze's default view layer is made up of simple [Blade templates](https://laravel.com/docs/10.x/blade) styled with [Tailwind CSS](https://tailwindcss.com/). Additionally, Breeze provides scaffolding options based on [Livewire](https://livewire.laravel.com/) or [Inertia](https://inertiajs.com/), with the choice of using Vue or React for the Inertia-based scaffolding.

![](https://laravel.com/img/docs/breeze-register.png)

#### Laravel Bootcamp

If you're new to Laravel, feel free to jump into the [Laravel Bootcamp](https://bootcamp.laravel.com/). The Laravel Bootcamp will walk you through building your first Laravel application using Breeze. It's a great way to get a tour of everything that Laravel and Breeze have to offer.

### [Installation](https://laravel.com/docs/10.x/starter-kits#laravel-breeze-installation)

First, you should [create a new Laravel application](https://laravel.com/docs/10.x/installation), configure your database, and run your [database migrations](https://laravel.com/docs/10.x/migrations). Once you have created a new Laravel application, you may install Laravel Breeze using Composer:

```
composer require laravel/breeze --dev
```

After Composer has installed the Laravel Breeze package, you may run the `breeze:install` Artisan command. This command publishes the authentication views, routes, controllers, and other resources to your application. Laravel Breeze publishes all of its code to your application so that you have full control and visibility over its features and implementation.

The `breeze:install` command will prompt you for your preferred frontend stack and testing framework:

```
php artisan breeze:install php artisan migratenpm installnpm run dev
```

### [Breeze & Blade](https://laravel.com/docs/10.x/starter-kits#breeze-and-blade)

The default Breeze "stack" is the Blade stack, which utilizes simple [Blade templates](https://laravel.com/docs/10.x/blade) to render your application's frontend. The Blade stack may be installed by invoking the `breeze:install` command with no other additional arguments and selecting the Blade frontend stack. After Breeze's scaffolding is installed, you should also compile your application's frontend assets:

```
php artisan breeze:install php artisan migratenpm installnpm run dev
```

Next, you may navigate to your application's `/login` or `/register` URLs in your web browser. All of Breeze's routes are defined within the `routes/auth.php` file.

> ![](https://laravel.com/img/callouts/lightbulb.min.svg)
> 
> To learn more about compiling your application's CSS and JavaScript, check out Laravel's [Vite documentation](https://laravel.com/docs/10.x/vite#running-vite).

### [Breeze & Livewire](https://laravel.com/docs/10.x/starter-kits#breeze-and-livewire)

Laravel Breeze also offers [Livewire](https://livewire.laravel.com/) scaffolding. Livewire is a powerful way of building dynamic, reactive, front-end UIs using just PHP.

Livewire is a great fit for teams that primarily use Blade templates and are looking for a simpler alternative to JavaScript-driven SPA frameworks like Vue and React.

To use the Livewire stack, you may select the Livewire frontend stack when executing the `breeze:install` Artisan command. After Breeze's scaffolding is installed, you should run your database migrations:

```
php artisan breeze:install php artisan migrate
```

### [Breeze & React / Vue](https://laravel.com/docs/10.x/starter-kits#breeze-and-inertia)

Laravel Breeze also offers React and Vue scaffolding via an [Inertia](https://inertiajs.com/) frontend implementation. Inertia allows you to build modern, single-page React and Vue applications using classic server-side routing and controllers.

Inertia lets you enjoy the frontend power of React and Vue combined with the incredible backend productivity of Laravel and lightning-fast [Vite](https://vitejs.dev/) compilation. To use an Inertia stack, you may select the Vue or React frontend stacks when executing the `breeze:install` Artisan command.

When selecting the Vue or React frontend stack, the Breeze installer will also prompt you to determine if you would like [Inertia SSR](https://inertiajs.com/server-side-rendering) or TypeScript support. After Breeze's scaffolding is installed, you should also compile your application's frontend assets:

```
php artisan breeze:install php artisan migratenpm installnpm run dev
```

Next, you may navigate to your application's `/login` or `/register` URLs in your web browser. All of Breeze's routes are defined within the `routes/auth.php` file.

### [Breeze & Next.js / API](https://laravel.com/docs/10.x/starter-kits#breeze-and-next)

Laravel Breeze can also scaffold an authentication API that is ready to authenticate modern JavaScript applications such as those powered by [Next](https://nextjs.org/), [Nuxt](https://nuxt.com/), and others. To get started, select the API stack as your desired stack when executing the `breeze:install` Artisan command:

```
php artisan breeze:install php artisan migrate
```

During installation, Breeze will add a `FRONTEND_URL` environment variable to your application's `.env` file. This URL should be the URL of your JavaScript application. This will typically be `http://localhost:3000` during local development. In addition, you should ensure that your `APP_URL` is set to `http://localhost:8000`, which is the default URL used by the `serve` Artisan command.

#### [Next.js Reference Implementation](https://laravel.com/docs/10.x/starter-kits#next-reference-implementation)

Finally, you are ready to pair this backend with the frontend of your choice. A Next reference implementation of the Breeze frontend is [available on GitHub](https://github.com/laravel/breeze-next). This frontend is maintained by Laravel and contains the same user interface as the traditional Blade and Inertia stacks provided by Breeze.

https://laravel.com/docs/10.x/starter-kits
## What Is Laravel Breeze

Laravel Breeze is an authentication scaffolding package for [Laravel](https://kinsta.com/topic/php-laravel/). Using it you can have a fully working login and registration system in minutes. It supports Blade, [Vue](https://kinsta.com/blog/vue-js/), and [React](https://kinsta.com/knowledgebase/what-is-react-js/) and also has an API version.

The main features of Laravel Breeze are:

- Login
- Registration
- Password reset
- Email verification
- Profile page, with editing

A commonly asked question can be when to choose Breeze and when to use [other Laravel authentication packages](https://kinsta.com/blog/laravel-authentication/#types-of-laravel-authentication-methods).

There are two similar packages in the Laravel ecosystem which can be confusing if you are new to this space.

The first one is [Laravel Fortify](https://kinsta.com/blog/laravel-authentication/#laravel-fortify) which is a headless authentication backend, making it ideal for building custom authentication systems without a pre-built UI.

Choose Fortify if you have very custom UI needs or if you are only responsible for the backend of the authentication.

The other package is [Laravel Jetstream](https://kinsta.com/blog/laravel-authentication/#laravel-jetstream) which offers a more advanced starting point for Laravel applications, including features like two-factor authentication and team management.

In contrast, Laravel Breeze is best suited for developers looking for a simple yet customizable authentication scaffold with support for various frontend frameworks and minimal overhead.

## Installing Laravel Breeze to a Fresh Laravel Project[](https://kinsta.com/blog/laravel-breeze/#installing-laravel-breeze-to-a-fresh-laravel-project)

To keep it simple, assume we already created a new Laravel project, if you need help with it you can follow our guide to [setup a new Laravel application at Kinsta](https://kinsta.com/docs/laravel-example-application/).

After that, we need to install Laravel Breeze with the following command:

```bash
composer require laravel/breeze --dev
```

In this tutorial, we will use Blade which is the default templating engine for Laravel. To start the scaffolding run these commands:

```bash
php artisan breeze:install blade
 
php artisan migrate
npm install
npm run dev
```

Laravel Breeze also has Vue / React / custom API versions, to use them you just need to put a flag in the command.

For Vue run:

```bash
php artisan breeze:install vue
```

For React run

```bash
php artisan breeze:install react
```

For custom API run

```bash
php artisan breeze:install api
```

After installing Laravel Breeze, you’ll notice that several files have been generated in your project directory. These files include [routes](https://kinsta.com/blog/laravel-routes/), controllers, and views that handle authentication, password reset, and email verification. You can explore these files and customize them to fit your application requirements.

## How To Customize the Registration Flow[](https://kinsta.com/blog/laravel-breeze/#how-to-customize-the-registration-flow)

The Laravel Breeze registration page comes with 4 predefined fields:

1. Name
2. Email
3. Password
4. Password confirmation

![Registration page predefined fields](https://kinsta.com/wp-content/uploads/2023/05/predefined-fields.png)

Registration page predefined fields

To extend the fields we’d like our registration form to feature, we need to open the `resources/views/auth/register.blade.php` file.

To continue with our example, we will make a phone field after the email field. To make this happen, add the following code after the email field:

```html
<div class="mt-4">
   <x-input-label for="phone" :value="__('Phone')" />
   <x-text-input id="phone" class="block mt-1 w-full" type="text" name="phone" :value="old('phone')" required autocomplete="phone" />
   <x-input-error :messages="$errors->get('phone')" class="mt-2" />
</div>
```

The phone field is now visible in the registration form.

![Phone field added](https://kinsta.com/wp-content/uploads/2023/05/phone-field-added.png)

Phone field added

## Modifying the Backend to Store the New Phone Field[](https://kinsta.com/blog/laravel-breeze/#modifying-the-backend-to-store-the-new-phone-field)

We now need to handle the new data in the backend. These require three steps: first, create and run a new migration, then add logic to the controller to store the data, and finally, add `phone` to the fillable properties in the `User` model.

Create a new migration that will add a phone field to our `users` table.

```bash
php artisan make:migration add_phone_field_to_users_table
```

Open the created file and add a string field called ‘phone’:

```php
Schema::table('users', function (Blueprint $table) {
   $table->string('phone')->nullable();
});
```

After that run the migration:

```bash
php artisan migrate
```

To store the phone field we need to modify the `RegisteredUserController.php`, in the `store` method make these modifications:

```php
$request->validate([
   'name' => ['required', 'string', 'max:255'],
   'email' => ['required', 'string', 'email', 'max:255', 'unique:'.User::class],
   ‘phone’ => [‘required’, ‘string’, ‘max:255’],
   'password' => ['required', 'confirmed', Rules\Password::defaults()],
]);

$user = User::create([
   'name' => $request->name,
   'email' => $request->email,
   ‘phone’ => $request->phone,
   'password' => Hash::make($request->password),
]);
```

Don’t forget to add the `phone` field to the fillable properties in the User model.

```php
protected $fillable = [
   'name',
   'email',
   'phone',
   'password',
];
```

That’s it, now we have the modified registration form!

## How To Enable Email Verification[](https://kinsta.com/blog/laravel-breeze/#how-to-enable-email-verification)

Email verification is the process of checking and authenticating emails that users have been provided in the registration form.

To enable this feature we need to implement `MustVerifyEmail` interface in our User model.

```php
use Illuminate\Contracts\Auth\MustVerifyEmail;
…

class User extends Authenticatable implements MustVerifyEmail
{
…
}
```

After that, an email will be sent out when a user registers with a link to verify their email.

However, we still need to add a middleware to our routes where we want to restrict access to unverified users.

We will create a new route called ‘only-verified’ and we will add ‘auth’ and ‘verified’ middleware. The auth middleware prevents access to guests and the verified middleware checks whether the user has verified their email.

Here is an example:

```php
Route::get('/only-verified', function () {
   return view('only-verified');
})->middleware(['auth', 'verified']);
```

https://kinsta.com/blog/laravel-breeze/

https://www.youtube.com/watch?v=3Jdy9rfYqN0