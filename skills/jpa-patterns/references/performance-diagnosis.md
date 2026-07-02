# Performance Diagnosis

Use this reference when a persistence path is slow, chatty, or memory-heavy.

## Diagnose before changing

Check:

- query count
- fetch plan
- result size
- pagination
- index coverage
- transaction scope
- whether the caller really needs entities

## N+1

Typical symptom:

```java
List<OrderEntity> orders = orderRepository.findAll();
for (OrderEntity order : orders) {
    order.getCustomer().getName();
}
```

Fix options:

1. fetch join for entity-loading use case
2. projection for read-only use case
3. `@EntityGraph` when it keeps the repository contract clearer

## Pagination

Always question unbounded list queries on endpoints, jobs, or exports.

```java
Pageable pageable = PageRequest.of(0, 50, Sort.by("createdAt").descending());
Page<OrderEntity> page = orderRepository.findByStatus(status, pageable);
```

## Batch work

For large write sets:

- configure Hibernate batch size
- flush and clear intentionally
- verify callbacks or auditing expectations on bulk operations

```java
for (int i = 0; i < entities.size(); i++) {
    repository.save(entities.get(i));
    if ((i + 1) % 25 == 0) {
        entityManager.flush();
        entityManager.clear();
    }
}
```

## Read-only tuning

Use `@Transactional(readOnly = true)` on read services or methods that do not mutate state.

## Index awareness

When a query is on a hot path, ask:

- what filters are used together
- what ordering is used
- whether the index matches the predicate and sort shape

## Useful dev-time logging

Use SQL logging only as proof, not as a permanent default:

```properties
logging.level.org.hibernate.SQL=DEBUG
logging.level.org.hibernate.orm.jdbc.bind=TRACE
```

## Quick checklist

- projection instead of entity?
- fetch join only where needed?
- pagination present?
- index implied by query?
- collection traversal inside loops?
- large export or batch split into chunks?
