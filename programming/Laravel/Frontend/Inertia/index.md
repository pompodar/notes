### [Inertia](https://laravel.com/docs/10.x/frontend#inertia)

Thankfully, Laravel offers the best of both worlds. [Inertia](https://inertiajs.com/) bridges the gap between your Laravel application and your modern Vue or React frontend, allowing you to build full-fledged, modern frontends using Vue or React while leveraging Laravel routes and controllers for routing, data hydration, and authentication — all within a single code repository. With this approach, you can enjoy the full power of both Laravel and Vue / React without crippling the capabilities of either tool.

After installing Inertia into your Laravel application, you will write routes and controllers like normal. However, instead of returning a Blade template from your controller, you will return an Inertia page:

```
<?php namespace App\Http\Controllers; use App\Http\Controllers\Controller;use App\Models\User;use Inertia\Inertia;use Inertia\Response; class UserController extends Controller{    /**     * Show the profile for a given user.     */    public function show(string $id): Response    {        return Inertia::render('Users/Profile', [            'user' => User::findOrFail($id)        ]);    }}
```

An Inertia page corresponds to a Vue or React component, typically stored within the `resources/js/Pages` directory of your application. The data given to the page via the `Inertia::render` method will be used to hydrate the "props" of the page component:

```
<script setup>import Layout from '@/Layouts/Authenticated.vue';import { Head } from '@inertiajs/vue3'; const props = defineProps(['user']);</script> <template>    <Head title="User Profile" />     <Layout>        <template #header>            <h2 class="font-semibold text-xl text-gray-800 leading-tight">                Profile            </h2>        </template>         <div class="py-12">            Hello, {{ user.name }}        </div>    </Layout></template>
```

As you can see, Inertia allows you to leverage the full power of Vue or React when building your frontend, while providing a light-weight bridge between your Laravel powered backend and your JavaScript powered frontend.

#### Server-Side Rendering

If you're concerned about diving into Inertia because your application requires server-side rendering, don't worry. Inertia offers [server-side rendering support](https://inertiajs.com/server-side-rendering). And, when deploying your application via [Laravel Forge](https://forge.laravel.com/), it's a breeze to ensure that Inertia's server-side rendering process is always running.

### [Starter Kits](https://laravel.com/docs/10.x/frontend#inertia-starter-kits)

If you would like to build your frontend using Inertia and Vue / React, you can leverage our Breeze or Jetstream [starter kits](https://laravel.com/docs/10.x/starter-kits#breeze-and-inertia) to jump-start your application's development. Both of these starter kits scaffold your application's backend and frontend authentication flow using Inertia, Vue / React, [Tailwind](https://tailwindcss.com/), and [Vite](https://vitejs.dev/) so that you can start building your next big idea.

https://laravel.com/docs/10.x/frontend


Inertia is a new approach to building classic server-driven web apps. We call it the modern monolith.

Inertia allows you to create fully client-side rendered, single-page apps, without the complexity that comes with modern SPAs. It does this by leveraging existing server-side patterns that you already love.

Inertia has no client-side routing, nor does it require an API. Simply build controllers and page views like you've always done! Inertia works great with any backend framework, but it's fine-tuned for [Laravel](https://laravel.com/).

## Not a framework

Inertia isn't a framework, nor is it a replacement for your existing server-side or client-side frameworks. Rather, it's designed to work with them. Think of Inertia as glue that connects the two. Inertia does this via adapters. We currently have three official client-side adapters (React, Vue, and Svelte) and two server-side adapters (Laravel and Rails).

https://inertiajs.com/

# Who is Inertia.js for?

Inertia was crafted for development teams and solo hackers who typically build server-side rendered applications using frameworks like Laravel, Ruby on Rails, or Django. You're used to creating controllers, retrieving data from the database (via an ORM), and rendering views.

But what happens when you want to replace your server-side rendered views with a modern, JavaScript-based single-page application frontend? The answer is always "you need to build an API". Because that's how modern SPAs are built.

This means building a REST or GraphQL API. It means figuring out authentication and authorization for that API. It means client-side state management. It means setting up a new Git repository. It means a more complicated deployment strategy. And this list goes on. It's a complete paradigm shift, and often a complete mess. We think there is a better way.

**Inertia empowers you to build a modern, JavaScript-based single-page application without the tiresome complexity.**

Inertia works just like a classic server-side rendered application. You create controllers, you get data from the database (via your ORM), and you render views. But, Inertia views are JavaScript page components written in React, Vue, or Svelte.

This means you get all the power of a client-side application and modern SPA experience, but you don't need to build an API. We think it's a breath of fresh air that will supercharge your productivity.

https://inertiajs.com/who-is-it-for


# Server-side setup

The first step when installing Inertia is to configure your server-side framework. Inertia maintains an official server-side adapter for [Laravel](https://laravel.com/). For other frameworks, please see the [community adapters](https://inertiajs.com/community-adapters).

Inertia is fine-tuned for Laravel, so the documentation examples on this website utilize Laravel. For examples of using Inertia with other server-side frameworks, please refer to the framework specific documentation maintained by that adapter.

## Laravel starter kits

Laravel's [starter kits](https://laravel.com/docs/starter-kits), Breeze and Jetstream, provide out-of-the-box scaffolding for new Inertia applications. These starter kits are the absolute fastest way to start building a new Inertia project using Laravel and Vue or React. However, if you would like to manually install Inertia into your application, please consult the documentation below.

## Install dependencies

First, install the Inertia server-side adapter using the Composer package manager.

```
composer require inertiajs/inertia-laravel
```

## Root template

Next, setup the root template that will be loaded on the first page visit to your application. This will be used to load your site assets (CSS and JavaScript), and will also contain a root `<div>` in which to boot your JavaScript application.

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
    @vite('resources/js/app.js')
    @inertiaHead
  </head>
  <body>
    @inertia
  </body>
</html>
```

This template should include your assets, as well as the `@inertia` and `@inertiaHead` directives.

By default, Inertia's Laravel adapter will assume your root template is named `app.blade.php`. If you would like to use a different root view, you can change it using the `Inertia::setRootView()` method.

## Middleware

Next we need to setup the Inertia middleware. You can accomplish this by publishing the `HandleInertiaRequests` middleware to your application, which can be done using the following Artisan command.

```
php artisan inertia:middleware
```

Once the middleware has been published, register the `HandleInertiaRequests` middleware in your `App\Http\Kernel` as the last item in your `web` middleware group.

```
'web' => [
    // ...
    \App\Http\Middleware\HandleInertiaRequests::class,
],
```

This middleware provides a `version()` method for setting your [asset version](https://inertiajs.com/asset-versioning), as well as a `share()` method for defining [shared data](https://inertiajs.com/shared-data).

## Creating responses

That's it, you're all ready to go server-side! Now you're ready to start creating Inertia [pages](https://inertiajs.com/pages) and rendering them via [responses](https://inertiajs.com/responses).

```
use Inertia\Inertia;

class EventsController extends Controller
{
    public function show(Event $event)
    {
        return Inertia::render('Event/Show', [
            'event' => $event->only(
                'id',
                'title',
                'start_date',
                'description'
            ),
        ]);
    }
}
```

https://inertiajs.com/server-side-setup


# Client-side setup

Once you have your [server-side framework configured](https://inertiajs.com/server-side-setup), you then need to setup your client-side framework. Inertia currently provides support for React, Vue, and Svelte.

## Laravel starter kits

Laravel's [starter kits](https://laravel.com/docs/starter-kits), Breeze and Jetstream, provide out-of-the-box scaffolding for new Inertia applications. These starter kits are the absolute fastest way to start building a new Inertia project using Laravel and Vue or React. However, if you would like to manually install Inertia into your application, please consult the documentation below.

## Install dependencies

First, install the Inertia client-side adapter corresponding to your framework of choice.

Vue 2Vue 3ReactSvelte

```
npm install @inertiajs/react
```

## Initialize the Inertia app

Next, update your main JavaScript file to boot your Inertia app. To accomplish this, we'll initialize the client-side framework with the base Inertia component.

Vue 2Vue 3ReactSvelte

```
import { createInertiaApp } from '@inertiajs/react'
import { createRoot } from 'react-dom/client'

createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.jsx', { eager: true })
    return pages[`./Pages/${name}.jsx`]
  },
  setup({ el, App, props }) {
    createRoot(el).render(<App {...props} />)
  },
})
```

The `setup` callback receives everything necessary to initialize the client-side framework, including the root Inertia `App` component.

## Resolving components

The `resolve` callback tells Inertia how to load a page component. It receives a page name (string), and returns a page component module. How you implement this callback depends on which bundler (Vite or Webpack) you're using.

Vue 2Vue 3ReactSvelte

```
// Vite
resolve: name => {
  const pages = import.meta.glob('./Pages/**/*.jsx', { eager: true })
  return pages[`./Pages/${name}.jsx`]
},

// Webpack
resolve: name => require(`./Pages/${name}`),
```

By default we recommend eager loading your components, which will result in a single JavaScript bundle. However, if you'd like to lazy-load your components, see our [code splitting](https://inertiajs.com/code-splitting) documentation.

## Defining a root element

By default, Inertia assumes that your application's root template has a root element with an `id` of `app`. If your application's root element has a different `id`, you can provide it using the `id` property.

```
createInertiaApp({
  id: 'my-app',
  // ...
})
```

https://inertiajs.com/client-side-setup

