# N+1 Query Problem

## What is the N+1 Query Problem?

The **N+1 Query Problem** happens when an application first retrieves a list of records (**1 query**), and then executes **one additional database query for each record** (**N queries**).

As the number of records increases, the number of database queries also increases, resulting in slower response times.

---

## Example

Suppose we need to return a list of products together with whether each product has been used in an order.

### ❌ Inefficient Approach

```java
List<Product> products = productRepository.findAll();

for (Product product : products) {
    boolean used = orderRepository.existsByProductId(product.getId());
    product.setCanDelete(!used);
}
```

If there are **100 products**, the application executes:

- 1 query to load all products
- 100 queries to check each product

**Total = 101 database queries**

```
findAllProducts()

↓

Product 1  → existsByProductId(1)
Product 2  → existsByProductId(2)
Product 3  → existsByProductId(3)
...
Product 100 → existsByProductId(100)
```

This is called the **N+1 Query Problem**.

---

## Why is this a problem?

Every database query requires:

- Sending a request to the database
- Database processing
- Returning the result

Executing hundreds of small queries is much slower than executing a single bulk query.

As your data grows, the response time grows too.

---

## Better Approach

Instead of querying one product at a time:

1. Retrieve all products.
2. Collect all product IDs.
3. Execute **one query** using all IDs.
4. Store the returned IDs in a `HashSet`.
5. Update each product using the `HashSet`.

### ✅ Efficient Approach

```java
List<Product> products = productRepository.findAll();

List<Long> productIds = ...;

Set<Long> usedProductIds =
    new HashSet<>(orderRepository.findUsedProductIds(productIds));

for (Product product : products) {
    product.setCanDelete(!usedProductIds.contains(product.getId()));
}
```

Now the database executes only:

- 1 query to load products
- 1 query to load all used product IDs

**Total = 2 database queries**

```
findAllProducts()

↓

findUsedProductIds([1,2,3,...,100])

↓

Update products in memory
```

---

## Benefits

- ✅ Eliminates the N+1 Query Problem
- ✅ Reduces database round trips
- ✅ Faster response time
- ✅ Better scalability
- ✅ Cleaner service-layer code

---

## Best Practice

Whenever you notice code like this:

```java
for (...) {
    repository.someMethod(...);
}
```

Ask yourself:

> "Can I fetch everything in one query instead?"

If the answer is **Yes**, prefer the bulk-query approach.
