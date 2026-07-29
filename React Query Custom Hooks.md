# React Query Custom Hooks

## Why Do We Use React Query?

When building React applications, we frequently communicate with a backend to fetch or modify data.

Without React Query, developers have to manually handle:

- Fetching data from APIs
- Loading states
- Error handling
- Client-side caching
- Refreshing data after updates
- Keeping the UI synchronized with the server

React Query simplifies all of these responsibilities by providing a powerful and consistent way to manage server state.

---

## What Problems Does React Query Solve?

React Query helps your application automatically manage:

- ✅ Data fetching
- ✅ Client-side caching
- ✅ Loading states
- ✅ Error handling
- ✅ Automatic background re-fetching
- ✅ Data synchronization
- ✅ Create, Update and Delete operations
- ✅ Keeping the UI up-to-date after data changes

Instead of writing the same logic repeatedly, React Query handles most of it for you.

---

# Types of Custom Hooks

Most projects organize React Query into two categories of custom hooks:

- Fetching Hooks (`useQuery`)
- Mutation Hooks (`useMutation`)

---

# 1. Fetching Hooks (`useQuery`)

## Purpose

Fetching hooks retrieve data from the backend.

Typical examples include:

- Get all users
- Get products
- Get quotation list
- Get student details

Example:

```tsx
const { data, isLoading, error } = useUsers();
```

---

## What Happens Internally?

A fetching hook typically:

1. Sends a GET request to the backend.
2. Stores the response in the React Query cache.
3. Returns:
   - data
   - loading state
   - error state
4. Automatically re-fetches data when it becomes stale.
5. Shares cached data between components.

---

## Why Is This Useful?

Without React Query, developers usually write something like:

```tsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(true);

useEffect(() => {
    fetchUsers().then((response) => {
        setData(response);
        setLoading(false);
    });
}, []);
```

This pattern gets repeated throughout the application.

React Query removes this boilerplate and provides a cleaner solution.

---

# 2. Mutation Hooks (`useMutation`)

## Purpose

Mutation hooks modify data on the server.

Typical operations include:

- Create
- Update
- Delete
- Approve
- Reject
- Restore

Example:

```tsx
const createUser = useCreateUser();

createUser.mutate(userData);
```

---

## What Happens Internally?

A mutation hook:

1. Sends a POST, PUT, PATCH, or DELETE request.
2. Tracks loading and error states.
3. Detects whether the request succeeds or fails.
4. Refreshes related data after a successful operation.

---

## Keeping the UI Up-to-Date

Suppose a user edits a product.

The product list should display the updated information immediately.

Instead of manually fetching the list again, React Query allows us to invalidate cached data.

Example:

```tsx
queryClient.invalidateQueries({
    queryKey: ["products"],
});
```

React Query automatically fetches fresh data, ensuring the UI stays synchronized with the backend.

---

# Why Do We Create Custom Hooks?

Although React Query provides `useQuery()` and `useMutation()`, most projects wrap them inside custom hooks.

Instead of writing this in every component:

```tsx
useQuery({
    queryKey: ["user", id],
    queryFn: () => getUser(id),
});
```

We create a reusable hook:

```tsx
const { data, isLoading } = useUser(id);
```

---

## Benefits of Custom Hooks

### 1. Reusable Logic

The API request logic is written once and reused everywhere.

---

### 2. Cleaner Components

Components only focus on displaying data.

Instead of dozens of lines of API-related code:

```tsx
const { data, isLoading } = useUser(id);
```

The component remains small, readable, and easier to maintain.

---

### 3. Centralized Error Handling

Shared behaviors such as error handling, retry logic, and authentication can be managed in one place.

---

### 4. Easier Maintenance

If an API endpoint changes, only the custom hook needs to be updated.

All components using that hook continue working without modification.

---

### 5. Centralized Cache Management

Invalidating or refreshing cached data is managed inside the hook instead of being repeated throughout the application.

---

# Typical Project Structure

```text
hooks/
│
├── queries/
│   ├── useUsers.ts
│   ├── useProducts.ts
│   └── useOrders.ts
│
└── mutations/
    ├── useCreateUser.ts
    ├── useUpdateUser.ts
    ├── useDeleteUser.ts
    └── useRestoreUser.ts
```

This structure keeps fetching logic and mutation logic organized and easy to locate.

---

# Summary

| Hook Type | Purpose | Common HTTP Methods |
|-----------|---------|---------------------|
| `useQuery` | Retrieve data from the backend | GET |
| `useMutation` | Create, update, or delete data | POST, PUT, PATCH, DELETE |

---

# Best Practices

- ✅ Keep API calls inside custom hooks.
- ✅ Keep components focused on rendering UI.
- ✅ Use meaningful query keys.
- ✅ Invalidate related queries after successful mutations.
- ✅ Let React Query manage caching instead of creating your own global cache.
- ✅ Reuse hooks throughout the application instead of duplicating API logic.

---

# Key Takeaway

> **Custom React Query hooks provide a clean, reusable, and scalable way to manage communication between your React application and the backend. They automatically handle data fetching, caching, loading states, error handling, and data synchronization, allowing developers to write less code while keeping the UI consistently up-to-date.**
