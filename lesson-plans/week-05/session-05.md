# Week 05 — Session 05: Database Relationships & Advanced Queries

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 05 Index: ../week-05.md
- Prev: ../../week-04/session-04.md
- Next: ../../week-06/session-06.md

**Session length:** 120 minutes

## Pre-class Checklist

- Basic CRUD operations working
- Understanding of Prisma basics
- Database with at least one model

## Learning Objectives

- Understand and implement database relationships
- Use Prisma to query related data
- Filter, sort, and paginate data
- Optimize queries for performance
- Handle nested data in API responses

## Key Terms

- Relationship, one-to-many, many-to-many, foreign key, include, select, filter, pagination

## You Will Build

- Database relationships in Prisma
- APIs that return related data
- Filtering and pagination

---

## 1) Database Relationships (30 minutes)

### Types of Relationships

**One-to-Many:** One user has many posts
**Many-to-Many:** Posts have many tags, tags have many posts
**One-to-One:** User has one profile

### One-to-Many Example

```prisma
model User {
  id    Int    @id @default(autoincrement())
  email String @unique
  posts Post[]
}

model Post {
  id        Int    @id @default(autoincrement())
  title     String
  content   String
  authorId  Int
  author    User   @relation(fields: [authorId], references: [id])
}
```

**AI-Assisted Learning:**
Ask your AI: "Explain database relationships (one-to-many, many-to-many) with examples. Generate Prisma schema examples."

---

## 2) Querying Related Data (30 minutes)

### Using Include

```typescript
// Get user with all posts
const user = await this.prisma.user.findUnique({
  where: { id: 1 },
  include: {
    posts: true,
  },
});
```

### Using Select

```typescript
// Select specific fields
const user = await this.prisma.user.findUnique({
  where: { id: 1 },
  select: {
    id: true,
    email: true,
    posts: {
      select: {
        title: true,
        createdAt: true,
      },
    },
  },
});
```

**AI-Assisted Learning:**
Ask your AI: "Generate Prisma query examples for querying related data with include and select. Explain when to use each."

---

## 3) Filtering and Sorting (20 minutes)

### Filtering

```typescript
// Find posts where author is user 1
const posts = await this.prisma.post.findMany({
  where: {
    authorId: 1,
  },
});

// Find posts with title containing "Nest"
const posts = await this.prisma.post.findMany({
  where: {
    title: {
      contains: 'Nest',
    },
  },
});
```

### Sorting

```typescript
// Sort by createdAt descending
const posts = await this.prisma.post.findMany({
  orderBy: {
    createdAt: 'desc',
  },
});
```

**AI-Assisted Learning:**
Ask your AI: "Generate Prisma filtering and sorting examples for [your use case]."

---

## 4) Pagination (20 minutes)

### Basic Pagination

```typescript
const page = 1;
const pageSize = 10;
const skip = (page - 1) * pageSize;

const posts = await this.prisma.post.findMany({
  skip,
  take: pageSize,
  orderBy: {
    createdAt: 'desc',
  },
});
```

### Pagination with Count

```typescript
const [posts, total] = await Promise.all([
  this.prisma.post.findMany({
    skip,
    take: pageSize,
  }),
  this.prisma.post.count(),
]);

return {
  data: posts,
  total,
  page,
  pageSize,
  totalPages: Math.ceil(total / pageSize),
};
```

**AI-Assisted Learning:**
Ask your AI: "Generate a pagination helper function for NestJS with Prisma. Include metadata in the response."

---

## 5) Guided Lab (20 minutes)

**Goal:** Add relationships to your schema and query related data.

**Steps:**
1. Add relationship to your Prisma schema
2. Run migration
3. Update service to query related data
4. Update controller to return nested data
5. Test with Postman

**Checklist:**
- [ ] Relationship defined in schema
- [ ] Migration run successfully
- [ ] Queries include related data
- [ ] Filtering and sorting implemented
- [ ] Pagination added (optional)

---

## Knowledge Checks

- Q: What's the difference between `include` and `select` in Prisma?
- Q: How do you implement one-to-many relationships in Prisma?
- Q: Why is pagination important?

---

## Common Pitfalls

- **Circular relationships:** Be careful with mutual includes
- **N+1 queries:** Use `include` to fetch related data in one query
- **Missing foreign keys:** Ensure relationships are properly defined

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm having trouble with Prisma relationships. [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Add Relationships

1. Add a relationship to your existing schema (e.g., User-Post, Category-Product)
2. Implement queries that return related data
3. Add filtering and sorting
4. Implement pagination

**Acceptance Criteria:**
- [ ] Relationship added and migrated
- [ ] Related data queried correctly
- [ ] Filtering/sorting working
- [ ] Pagination implemented

---

## Navigation

- Back: ../learning-plan.md
- Week 05 Index: ../week-05.md
- Prev: ../../week-04/session-04.md
- Next: ../../week-06/session-06.md

