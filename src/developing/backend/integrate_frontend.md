# Integrating the backend/frontend

The database is the only place you can securely store private information. Everything stored in the repositories can easily be accessed by anyone. In the future, we might want to make an easier way to store private content.

So, if you want to display some secret information on the backend on the frontend, you will need to load it using an HTTP request. To make this easier, we use two libraries, `TanStack Query` and `ky`. 

## A query

A "query" is basically an automatic function that, once the page loads, will load whatever function you ask it to and keep it up to date. It can be enabled based on whether or not someone is authenticated. 

An example of of a query is (defined in `src/functions/queries.ts`):

```typescript
export const useAdminKlassementQuery = (
  au: AuthUse,
  rank_type: "points" | "training",
) =>
  useQuery(
    [`tr_klass_admin_${rank_type}`],
    () => klassement_request(au, true, rank_type),
    {
      staleTime: longStaleTime,
      cacheTime: longCacheTime,
      enabled: au.authState.isAuthenticated,
    },
  );

```

Here, `useQuery` is a function from the TanStack Query library, while `klassement_request` is defined by us. Here it is important that the `tr_klass_admin_${rank_type}` key is unique, otherwise the caches will not work correctly.

Let's look at `klassement_request` (inside `src/functions/api/klassementen.ts`):

```typescript
export const klassement_request = async (
  auth: AuthUse,
  is_admin: boolean,
  rank_type: "points" | "training",
  options?: Options,
): Promise<KlassementList> => {
    
    # ... ommitted for brevity

  let response = await back_request(
    `${role}/classification/${rank_type}/`,
    auth,
    options,
  );

  const punt_klas: KlassementList = KlassementList.parse(response);

  # ... ommitted for brevity

  return punt_klas;
};

```

Here, `back_request` is a function we defined, which calls the backend using the `ky` library. Basically, this is just a simple GET request, which we then parse using `zod` (the `.parse` part). The backend will check the information in the `auth` part, returning the data if you have the right scope and are the right user.

How is the result of this query used now? Let's see (in `src/pages/Admin/components/Klassement.tsx`):

```tsx
const q = useAdminKlassementQuery({ authState, setAuthState }, typeName);
const pointsData = queryError(
  q,
  defaultTraining,
  `Class ${typeName} Query Error`,
);
```

The `pointsData` now simply contains the data you want. All the data loading happens in the background. `queryError` is also defined by us and ensures that any potential error is properly caught. If the data is still loading, it will display the default data instead (`defaultTraining`) in this case. In the future we might want to make sure this is displayed in a nicer way in the UI. Because right now, it will first show the default data, before flickering and switching to the loaded data once it comes in.