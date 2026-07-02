# Query And Projection Patterns

Use this reference when deciding how to implement a read or write query path.

## Choose the pattern

| Need | Pattern |
|---|---|
| simple lookup on one or two fields | derived query |
| join, readable multi-filter query, fetch plan control | `@Query` |
| read-only response with limited columns | DTO or record projection |
| dynamic criteria or `EntityManager` work | custom repository |
| read model separate from write model | query service |

## Derived query

Use for trivial lookups only.

```java
Optional<CustomerEntity> findByEmail(String email);
List<CustomerEntity> findByStatus(Status status);
```

## `@Query`

Use when query intent is no longer obvious from a method name.

```java
@Query("""
    select distinct o
    from OrderEntity o
    left join fetch o.items
    where o.customerId = :customerId
    and o.status in :statuses
    order by o.createdAt desc
    """)
List<OrderEntity> findRecentOrders(
    @Param("customerId") Long customerId,
    @Param("statuses") List<OrderStatus> statuses);
```

Guardrails:

- use text blocks for non-trivial JPQL
- use `@Param` explicitly
- add `distinct` when fetch-joining collections

## DTO or record projection

Prefer for read-only API or export responses.

```java
public record OrderSummary(
    Long id,
    String orderNumber,
    String customerName,
    BigDecimal total
) {}

@Query("""
    select new com.example.orders.OrderSummary(
        o.id, o.orderNumber, c.name, o.total
    )
    from OrderEntity o
    join o.customer c
    where o.status = :status
    """)
List<OrderSummary> findSummaries(@Param("status") OrderStatus status);
```

## Custom repository

Use when:

- filters are dynamic
- bulk updates or deletes are required
- `EntityManager` control is necessary

```java
public interface OrderRepositoryCustom {
    List<OrderEntity> search(OrderSearchCriteria criteria);
}
```

Important:

- bulk operations bypass entity callbacks
- flush and clear intentionally after large bulk changes

## Query service

Use when the read model is materially different from the write model.

```java
@Service
@Transactional(readOnly = true)
public class OrderQueryService {

    private final JdbcTemplate jdbcTemplate;

    public OrderQueryService(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
}
```

## Save semantics

Do not assume `save()` is harmless:

- confirm whether the entity is new, managed, detached, or loaded elsewhere
- confirm whether write order or flush timing matters
- avoid hiding non-trivial state transitions in repository convenience methods
