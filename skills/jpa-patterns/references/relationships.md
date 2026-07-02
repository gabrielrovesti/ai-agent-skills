# Relationships

Use this reference when relationship shape or fetch behavior is part of the problem.

## General rules

- Default to `LAZY`.
- Prefer `@ManyToOne` over parent-owned collections when possible.
- Treat `@ManyToMany` as a design smell unless proven otherwise.
- Keep both sides of a bidirectional relationship consistent only when the relationship is actually justified.

## `@ManyToOne`

Usually the best default.

```java
@ManyToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "order_id", nullable = false)
private OrderEntity order;
```

Use only the foreign ID instead when navigation is not needed:

```java
@Column(name = "product_id", nullable = false)
private Long productId;
```

## `@OneToMany`

Use only when parent lifecycle truly owns the children and collection size stays reasonable.

```java
@OneToMany(
    mappedBy = "order",
    cascade = CascadeType.ALL,
    orphanRemoval = true,
    fetch = FetchType.LAZY
)
private List<OrderItemEntity> items = new ArrayList<>();
```

Prefer querying the child table directly when:

- collection size can grow
- filtering is needed
- parent loading would be heavy

## `@OneToOne`

Use sparingly. Prefer unidirectional mapping and validate whether a shared primary key model is a better fit.

## `@ManyToMany`

Prefer a join entity instead.

```java
@Entity
public class EnrollmentEntity {

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    private StudentEntity student;

    @ManyToOne(fetch = FetchType.LAZY, optional = false)
    private CourseEntity course;
}
```

## Fetch strategy choices

- use fetch join for one specific read path
- use projection when caller needs only part of the graph
- do not “fix” lazy problems by broad `EAGER` usage

## Signs the mapping is wrong

- helper methods required everywhere just to keep both sides aligned
- serialization accidentally walks the full graph
- API layer depends on entity shape
- small use cases force loading large collections
