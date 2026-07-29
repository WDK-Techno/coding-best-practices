# Why Use `HashSet` Instead of `List` for Lookups?

Sometimes we only need to answer one simple question:

> "Does this ID exist?"

In this situation, using a **HashSet** is usually much faster than using a **List**.

---

## Example

Suppose the database returns the following IDs:

```text
[101, 102, 103, 104, 105]
```

### Using a List

```java
List<Long> ids = List.of(101L, 102L, 103L, 104L, 105L);

ids.contains(105L);
```

To find `105`, Java checks the list one item at a time.

```
101 ❌

102 ❌

103 ❌

104 ❌

105 ✅
```

The larger the list becomes, the more items Java may need to check.

This is called **O(n)** time complexity.

---

## What does O(n) mean?

Imagine searching for a name in a notebook.

If the name is on the last page, you may need to read every page before finding it.

The more pages there are, the longer it takes.

That is exactly how a `List.contains()` works.

---

## Using a HashSet

```java
Set<Long> ids = Set.of(101L, 102L, 103L, 104L, 105L);

ids.contains(105L);
```

A `HashSet` does **not** search item by item.

Instead, it uses a hashing mechanism to jump directly to where the value should be stored.

```
105 ✅
```

This is called **O(1)** time complexity.

---

## What does O(1) mean?

Think of a dictionary.

You don't read every page to find a word.

Instead, you jump directly to the correct section.

A `HashSet` works in a similar way.

Whether it stores:

- 10 items
- 1,000 items
- 100,000 items

the lookup time remains almost the same.

---

## Performance Comparison

| Collection | Lookup (`contains`) | How it works |
|------------|--------------------:|--------------|
| `List` | O(n) | Checks items one by one |
| `HashSet` | O(1) | Directly finds the item using hashing |

---

## When Should You Use a HashSet?

Use a `HashSet` when:

- You frequently call `contains()`
- You only need to know whether an item exists
- Duplicate values are not needed
- Fast lookup performance is important

---

## Common Example

Instead of:

```java
List<Long> usedIds = repository.findUsedIds(ids);

for (Product product : products) {
    if (usedIds.contains(product.getId())) {
        ...
    }
}
```

Prefer:

```java
Set<Long> usedIds =
    new HashSet<>(repository.findUsedIds(ids));

for (Product product : products) {
    if (usedIds.contains(product.getId())) {
        ...
    }
}
```

This becomes especially important when working with hundreds or thousands of records.

---

## Best Practice

If your code repeatedly calls:

```java
collection.contains(...)
```

consider whether the collection should be a **HashSet** instead of a **List**.

Using the right data structure can significantly improve performance with very little code change.
