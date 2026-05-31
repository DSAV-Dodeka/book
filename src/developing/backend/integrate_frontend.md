# Integrating the backend/frontend

The database is the only place you can securely store private information. Everything stored in the repositories can easily be accessed by anyone. In the future, we might want to make an easier way to store private content.

So, if you want to display private backend data on the frontend, load it with an HTTP request. The current frontend uses native `fetch` wrappers in `dodekafrontend/src/functions/backend.ts` and React Query hooks/options in `dodekafrontend/src/functions/query.ts`.

## A query

A "query" is basically an automatic function that, once the page loads, will load whatever function you ask it to and keep it up to date. It can be enabled based on whether or not someone is authenticated. 

Use the current files as the source of truth:

- Add low-level HTTP calls in `dodekafrontend/src/functions/backend.ts`.
- Add React Query hooks or `queryOptions` in `dodekafrontend/src/functions/query.ts`.
- For backend route names and permissions, check the route table in `dodeka/backend/src/apiserver/app.py`.
- For backend commands, ports, and auth details, prefer `dodeka/backend/README.md` over copying commands here.

Minimal pattern:

```ts
// backend.ts
export async function loadThing(): Promise<Thing> {
  const response = await get("/some/backend/route/", true);
  if (!response.ok) throw new Error(await response.text());
  return response.json();
}

// query.ts
export function useThing(enabled: boolean) {
  return useQuery({
    queryKey: ["thing"],
    queryFn: backend.loadThing,
    enabled,
  });
}
```
