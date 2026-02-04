# React and React Router

We use [React Router 7](https://reactrouter.com/) for client-side routing, which means code on the page itself handles navigation between subpages. This is also known as a "single-page application". For more details, see the section on [architecture](../../architecture/frontend.md).

## Routes

Routes are defined in `src/routes.ts` using React Router's configuration API.

### Basic route

```ts
import { type RouteConfig, route } from "@react-router/dev/routes";

export default [
  route("contact", "./pages/contact/contact.tsx"),
] satisfies RouteConfig;
```

This makes the page available at `/contact`.

### Layout with nested routes

Most routes use a shared layout (navigation bar, footer). Wrap routes in `layout()`:

```ts
import { type RouteConfig, layout, route, index } from "@react-router/dev/routes";

export default [
  layout("./pages/layout.tsx", [
    index("./pages/home/home.tsx"),           // Shows at /
    route("contact", "./pages/contact.tsx"),  // Shows at /contact
  ]),
] satisfies RouteConfig;
```

The layout component uses `<Outlet />` to render child routes:

```tsx
import { Outlet } from "react-router";

export default function AppLayout() {
  return (
    <div>
      <NavigationBar />
      <Outlet />  {/* Child routes render here */}
      <Footer />
    </div>
  );
}
```

### Route prefixes

Group related routes under a common path prefix:

```ts
import { prefix, route, index } from "@react-router/dev/routes";

...prefix("vereniging", [
  index("./pages/vereniging/vereniging.tsx"),      // /vereniging
  route("bestuur", "./pages/vereniging/bestuur.tsx"),  // /vereniging/bestuur
  route("commissies", "./pages/vereniging/commissies.tsx"),  // /vereniging/commissies
]),
```

### Dynamic routes

Use `:param` for dynamic segments:

```ts
route(":wedstrijdPath", "./pages/wedstrijden/eigen/wedstrijd.tsx"),
```

Access the parameter in your component:

```tsx
import { useParams } from "react-router";

function Wedstrijd() {
  const { wedstrijdPath } = useParams();
  // ...
}
```

## Creating a new page

1. Create a folder in `src/pages/` with your page name (lowercase)
2. Add a `.tsx` file for the component and optionally a `.css`/`.scss` file for styles
3. Export a default function component
4. Add the route to `src/routes.ts`

Example page (`src/pages/example/example.tsx`):

```tsx
import PageTitle from "$components/PageTitle";
import "./example.css";

function Example() {
  return (
    <div>
      <PageTitle title="Example Page" />
      <p>Hello world!</p>
    </div>
  );
}

export default Example;
```

Then in `src/routes.ts`:

```ts
route("example", "./pages/example/example.tsx"),
```

## Adding to the navigation bar

Edit `src/components/Navigation Bar/NavigationBar.tsx` to add your page to the menu. Add it to both `navItems` (desktop) and `navMobileContainer` (mobile).

## Prerendering

Static pages are prerendered at build time for better performance and SEO. Configure prerendering in `react-router.config.ts`:

```ts
import type { Config } from "@react-router/dev/config";

export default {
  appDirectory: "src",
  ssr: false,  // No server-side rendering, just static prerendering
  async prerender() {
    return [
      "/",
      "/contact",
      "/vereniging",
      "/vereniging/bestuur",
      // ... add your static pages here
    ];
  },
} satisfies Config;
```

When you add a new static page, add its path to the `prerender()` array. Dynamic paths can be generated programmatically:

```ts
async prerender() {
  const dynamicPaths = getDynamicPaths().map((p) => `/prefix${p}`);
  return [
    "/",
    "/static-page",
    ...dynamicPaths,
  ];
}
```

Pages not in this list will still work but are rendered client-side only. Only add routes that don't change based on e.g. the user or other dynamic state.

## .tsx vs .jsx

For new components, prefer `.tsx`, which ensures proper TypeScript checking. This makes development easier by providing hints about available properties and prevents bugs.
