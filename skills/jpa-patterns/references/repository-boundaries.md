# Repository Boundaries

Use this reference when the main question is where persistence ownership belongs and what should or should not get a repository.

## Default stance

- Create repositories for aggregate roots, not automatically for every mapped entity.
- Keep write ownership close to the aggregate that enforces invariants.
- Prefer querying child rows from the many side instead of forcing parent-owned collections when lifecycle ownership is weak.

## Repository boundary checklist

- Which type owns the business invariant?
- Which type is created, updated, or deleted as a unit?
- Does the caller need navigation or only identifiers?
- Does exposing a child repository bypass aggregate rules?

## Patterns

### Aggregate-root repository

Use when:

- the entity is loaded and modified as the main business object
- child changes are part of the same invariant

```java
public interface OrderRepository extends JpaRepository<OrderEntity, Long> {
    Optional<OrderEntity> findByExternalId(String externalId);
}
```

### Query child rows from the many side

Use when:

- the parent collection can grow
- the caller needs filtered child data
- parent-owned collection traversal would encourage N+1 or oversize loads

```java
public interface OrderItemRepository extends JpaRepository<OrderItemEntity, Long> {
    List<OrderItemEntity> findByOrderId(Long orderId);
}
```

### Store foreign ID instead of entity reference

Use when:

- modules are loosely coupled
- related entity data is rarely needed
- avoiding navigation is simpler than managing fetch plans

```java
@Column(name = "product_id", nullable = false)
private Long productId;
```

## Smells

- repository for every table with no ownership rationale
- parent entity carrying large `@OneToMany` collections that are only read occasionally
- child repository added only to work around missing query design
- entity graph complexity caused by modeling navigation that the use case does not need
