# React and React Router

We use a concept called "client-side routing", which means that code on the page itself sends you to the subpages. This is also known as a "single-page application". For more details, see the section on [architecture](../../architecture/frontend.md).

## Routes

### Define a new route

To define a route, add a an element inside teh <Routes> element in `src/App.tsx`:

```tsx
<Route path="/vereniging" element={<Vereniging />} />
```

Here `<Vereniging />` is the React component that consists of the entire page. You will need to import it from the right file. Every single page, also every subpage, needs a separate route like this. `path="/vereniging"` indicates the path at which the page will be visible.

### Add it to the menu bar

The `src/components/Navigation Bar/NavigationBar.jsx` file contains all the different menu items. Don't forget to add it to both the navItems and the navMobileContainer.

## .tsx vs .jsx?

For new components, prefer `.tsx`, which ensures proper TypeScript checking happens, which can make development easier by providing hints about what properties are available and also prevents bugs.

## Authentication

We use React Context to make the `authState` available everywhere. Inside the component, simply put:

```tsx
const {authState, setAuthState} = useContext(authContext)
```

Then, by checking `authState.isLoaded && authState.isAuthenticated` you can check whether someone has been authenticated as a member and whether the route should be available. Note: any data that you store on the frontend is publicly available (either through the source code, but also using 'inspect element' in the browser)! So any sensitive data should be stored in the backend and retrieved using requests. See [the section on integrating the backend and frontend](../backend/integrate_frontend.md).

You can use `authState.scope.includes("<role>")` to check if someone has a role, but remember someone can just edit this code in the browser. So any information available on pages stored in the frontend repository should nto be sensitive. So it's fine to simply display the page skeleton based on checking the authState, but don't show private data based on that. 