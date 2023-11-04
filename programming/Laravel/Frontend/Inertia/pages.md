
When building applications using Inertia, each page in your application typically has its own controller / route and a corresponding JavaScript component. This allows you to retrieve just the data necessary for that page - no API required.

In addition, all of the data needed for the page can be retrieved before the page is ever rendered by the browser, eliminating the need for displaying "loading" states when users visit your application.

## Creating pages

Inertia pages are simply JavaScript components. If you have ever written a Vue, React, or Svelte component, you will feel right at home. As you can see in the example below, pages receive data from your application's controllers as props.

Vue 2Vue 3ReactSvelte

```
import Layout from './Layout'
import { Head } from '@inertiajs/react'

export default function Welcome({ user }) {
  return (
    <Layout>
      <Head title="Welcome" />
      <h1>Welcome</h1>
      <p>Hello {user.name}, welcome to your first Inertia app!</p>
    </Layout>
  )
}
```

Given the page above, you can render the page by returning an [Inertia response](https://inertiajs.com/responses) from a controller or route. In this example, let's assume this page is stored at `resources/js/Pages/User/Show.jsx` within a Laravel application.

```
use Inertia\Inertia;

class UserController extends Controller
{
    public function show(User $user)
    {
        return Inertia::render('User/Show', [
          'user' => $user
        ]);
    }
}
```

## Creating layouts

While not required, for most projects it makes sense to create a site layout that all of your pages can extend. You may have noticed in our page example above that we're wrapping the page content within a `<Layout>` component. Here's an example of such a component:

Vue 2Vue 3ReactSvelte

```
import { Link } from '@inertiajs/react'

export default function Layout({ children }) {
  return (
    <main>
      <header>
        <Link href="/">Home</Link>
        <Link href="/about">About</Link>
        <Link href="/contact">Contact</Link>
      </header>
      <article>{children}</article>
    </main>
  )
}
```

As you can see, there is nothing Inertia specific within this template. This is just a typical React component.

## Persistent layouts

While it's simple to implement layouts as children of page components, it forces the layout instance to be destroyed and recreated between visits. This means you cannot have persistent layout state when navigating between pages.

For example, maybe you have an audio player on a podcast website that you want to continue playing as users navigate the site. Or, maybe you simply want to maintain the scroll position in your sidebar navigation between page visits. In these situations, the solution is to leverage Inertia's persistent layouts.

Vue 2Vue 3ReactSvelte

```
import Layout from './Layout'

const Home = ({ user }) => {
  return (
    <>
      <H1>Welcome</H1>
      <p>Hello {user.name}, welcome to your first Inertia app!</p>
    </>
  )
}

Home.layout = page => <Layout children={page} title="Welcome" />

export default Home
```

You can also create more complex layout arrangements using nested layouts.

Vue 2Vue 3ReactSvelte

```
import SiteLayout from './SiteLayout'
import NestedLayout from './NestedLayout'

const Home = ({ user }) => {
  return (
    <>
      <H1>Welcome</H1>
      <p>Hello {user.name}, welcome to your first Inertia app!</p>
    </>
  )
}

Home.layout = page => (
  <SiteLayout title="Welcome">
    <NestedLayout children={page} />
  </SiteLayout>
)

export default Home
```

## Default layouts

If you're using persistent layouts, you may find it convenient to define the default page layout in the `resolve()` callback of your application's main JavaScript file.

Vue 2Vue 3ReactSvelte

```
import Layout from './Layout'

createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.jsx', { eager: true })
    let page = pages[`./Pages/${name}.jsx`]
    page.default.layout = page.default.layout || (page => <Layout children={page} />)
    return page
  },
  // ...
})
```

This will automatically set the page layout to `Layout` if a layout has not already been set for that page.

You can even go a step further and conditionally set the default page layout based on the page `name`, which is available to the `resolve()` callback. For example, maybe you don't want the default layout to be applied to your public pages.

Vue 2Vue 3ReactSvelte

```
import Layout from './Layout'

createInertiaApp({
  resolve: name => {
    const pages = import.meta.glob('./Pages/**/*.jsx', { eager: true })
    let page = pages[`./Pages/${name}.jsx`]
    page.default.layout = name.startsWith('Public/') ? undefined : page => <Layout children={page} />
    return page
  },
  // ...
})

https://inertiajs.com/pages