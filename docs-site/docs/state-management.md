---
sidebar_position: 3
---

# State Management

This document explains the state management approach used in the Crypto Price Tracker application.

## State Management with React Query

We chose [React Query](https://tanstack.com/query/latest) (TanStack Query) as our state management solution for several reasons:

1. **Server State Management**: React Query is specifically designed for managing server state (data fetched from APIs)
2. **Caching and Revalidation**: It provides built-in caching with configurable stale times
3. **Loading and Error States**: It handles loading and error states automatically
4. **Refetching Strategies**: It offers various refetching strategies (on window focus, on interval, etc.)
5. **Devtools**: It comes with excellent developer tools for debugging

## Implementation

### Query Client Setup

We set up React Query in our `QueryProvider.tsx` component:

```tsx
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { useState } from "react";

export default function QueryProvider({ children }) {
  const [queryClient] = useState(() => new QueryClient());

  return (
    <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
  );
}
```

### Query Configuration

In our `page.tsx` component, we configure the query:

```tsx
const {
  data: cryptoList,
  isLoading,
  isError,
} = useQuery({
  queryKey: ["cryptos"],
  queryFn: fetchCryptos,
  staleTime: 60000,
  refetchOnWindowFocus: false,
});
```

This configuration:

- Uses a query key of `['cryptoList']` to identify this query
- Uses our `fetchCryptos` function to fetch the data
- Sets a stale time of 60,000 milliseconds (1 minute)
- Automatically provides `isLoading`, `isError`, and other status indicators

### Local UI State

For UI-specific state that doesn't need to be persisted or shared widely, we use React's built-in `useState` hook:

```tsx
const [searchTerm, setSearchTerm] = useState("");
```

This approach keeps UI state simple and localized to the components that need it.

## State Management Architecture

Our state management architecture follows these principles:

1. **Server State vs. UI State**: We separate server state (managed by React Query) from UI state (managed by useState)
2. **Minimal State**: We keep state minimal and derive values when possible

## Benefits of Our Approach

Our state management approach provides several benefits:

1. **Simplified Data Fetching**: React Query handles the complexities of fetching, caching, and updating data
2. **Automatic Loading States**: Loading indicators are shown automatically when data is being fetched
3. **Error Handling**: Errors are caught and can be displayed to the user
4. **Optimized Performance**: React Query optimizes when components re-render
