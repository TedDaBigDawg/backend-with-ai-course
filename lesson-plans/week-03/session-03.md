# Week 03 — Session 03: PostgreSQL & Prisma Basics

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 03 Index: ../week-03.md
- Prev: ../../week-02/session-02.md
- Next: ../../week-04/session-04.md

**Session length:** 120 minutes

## Pre-class Checklist

- PostgreSQL installed locally or cloud database set up
- NestJS project from previous sessions ready
- Understanding of basic database concepts

## Learning Objectives

- Understand database schema and tables
- Set up Prisma in a NestJS project
- Design database schemas using Prisma
- Create and run database migrations
- Perform basic database queries with Prisma Client

## Key Terms

- Database, schema, table, model, migration, ORM, Prisma, PostgreSQL, query, relation

## You Will Build

- A database schema with Prisma
- Database migrations
- Basic queries using Prisma Client

---

## 1) Database Fundamentals (15 minutes)

### What is a Database?

A **database** is an organized collection of data stored electronically. Think of it as a digital filing cabinet.

### Why Use Databases?

- **Persistent storage:** Data survives application restarts
- **Efficient queries:** Find data quickly
- **Relationships:** Connect related data
- **Data integrity:** Ensure data is valid and consistent

### Types of Databases

**Relational Databases (SQL):**
- PostgreSQL, MySQL, SQLite
- Data stored in tables with relationships
- Uses SQL (Structured Query Language)

**NoSQL Databases:**
- MongoDB, Redis, DynamoDB
- Different data structures (documents, key-value, etc.)

**We're using PostgreSQL (Relational)**

**AI-Assisted Learning:**
Ask your AI: "Explain the difference between SQL and NoSQL databases using simple analogies. When would you use each?"

---

## 2) Introduction to Prisma (15 minutes)

### What is Prisma?

Prisma is a **modern ORM (Object-Relational Mapping)** for Node.js and TypeScript.

**What does ORM mean?**
- Maps database tables to code objects
- Lets you work with databases using TypeScript/JavaScript instead of SQL
- Provides type safety and autocompletion

### Why Prisma?

- **Type-safe:** Get autocompletion and error checking
- **Migrations:** Version control for database changes
- **Easy to use:** Simple API for database operations
- **Great developer experience:** Excellent tooling and documentation

**AI-Assisted Learning:**
Ask your AI: "Generate a Prisma schema example for a blog (User, Post, Comment) and explain each part."

---

## 3) Setting Up PostgreSQL (20 minutes)

### Option 1: Local Installation

1. Download PostgreSQL from [postgresql.org](https://www.postgresql.org/download/)
2. Install with default settings
3. Remember your password (you'll need it)
4. PostgreSQL runs on port 5432 by default

### Option 2: Cloud Database (Recommended for Beginners)

1. Sign up for [Neon](https://neon.tech/) or [Supabase](https://supabase.com/)
2. Create a new project
3. Copy your database connection string
4. No local installation needed!

### Verify Installation

```bash
psql --version
```

Or test connection to cloud database using the connection string.

**AI-Assisted Learning:**
If you have trouble with PostgreSQL setup, ask your AI: "I'm trying to set up PostgreSQL for NestJS. I'm getting this error: [error]. How do I fix it?"

---

## 4) Installing Prisma (10 minutes)

### Step 1: Install Prisma Dependencies

In your NestJS project:

```bash
npm install prisma @prisma/client
npm install -D prisma
```

### Step 2: Initialize Prisma

```bash
npx prisma init
```

This creates:
- `prisma/schema.prisma` - Your database schema
- `.env` - Environment variables (contains database URL)

### Step 3: Configure Database Connection

Edit `.env` file:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

For cloud databases, use the connection string provided by your provider.

**AI-Assisted Learning:**
Ask your AI: "Generate a .env.example file for a NestJS + Prisma project and explain what each variable does."

---

## 5) Creating Your First Schema (25 minutes)

### Understanding Prisma Schema

The `schema.prisma` file defines your database structure.

### Basic Schema Example

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

### Schema Explained

- **generator client:** Generates Prisma Client
- **datasource db:** Database configuration
- **model User:** Defines a database table
- **@id:** Primary key
- **@default(autoincrement()):** Auto-incrementing ID
- **@unique:** Ensures email is unique
- **String?:** Optional field (nullable)

### Creating Your Schema

**Guided Lab:** Create a simple schema for a blog

```prisma
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  published Boolean  @default(false)
  authorId  Int
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

**AI-Assisted Learning:**
Ask your AI: "I want to create a schema for [your use case]. Generate a Prisma schema and explain each field and decorator."

---

## 6) Running Migrations (15 minutes)

### What are Migrations?

**Migrations** are files that describe changes to your database structure. They allow you to:
- Version control your database
- Apply changes consistently
- Roll back if needed

### Creating Your First Migration

```bash
npx prisma migrate dev --name init
```

This:
1. Creates a migration file
2. Applies it to your database
3. Generates Prisma Client

### Applying Migrations

```bash
npx prisma migrate deploy
```

Use this in production to apply pending migrations.

### Generating Prisma Client

After schema changes:

```bash
npx prisma generate
```

This updates the Prisma Client with your new schema.

**AI-Assisted Learning:**
If migrations fail, ask your AI: "My Prisma migration failed with this error: [error]. What does it mean and how do I fix it?"

---

## 7) Using Prisma Client in NestJS (20 minutes)

### Setting Up Prisma Service

Create `prisma.service.ts`:

```typescript
import { Injectable, OnModuleInit } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit {
  async onModuleInit() {
    await this.$connect();
  }

  async onModuleDestroy() {
    await this.$disconnect();
  }
}
```

### Using Prisma in a Service

```typescript
import { Injectable } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Injectable()
export class UserService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    return this.prisma.user.findMany();
  }

  async findOne(id: number) {
    return this.prisma.user.findUnique({
      where: { id },
    });
  }

  async create(data: { email: string; name?: string }) {
    return this.prisma.user.create({
      data,
    });
  }
}
```

### Basic Prisma Queries

**Find all:**
```typescript
this.prisma.user.findMany()
```

**Find one:**
```typescript
this.prisma.user.findUnique({ where: { id: 1 } })
```

**Create:**
```typescript
this.prisma.user.create({ data: { email: 'test@example.com' } })
```

**Update:**
```typescript
this.prisma.user.update({
  where: { id: 1 },
  data: { name: 'New Name' }
})
```

**Delete:**
```typescript
this.prisma.user.delete({ where: { id: 1 } })
```

**AI-Assisted Learning:**
Ask your AI: "Generate Prisma query examples for [your model] including findMany with filters, create with relations, and update operations."

---

## Guided Lab (30 minutes)

**Goal:** Set up Prisma, create a schema, run migrations, and query data.

**Steps:**

1. Install Prisma in your NestJS project
2. Initialize Prisma and configure database connection
3. Create a schema with at least one model (e.g., User, Post, Product)
4. Run migration to create tables
5. Create a PrismaService in NestJS
6. Create a service that uses Prisma to query data
7. Create a controller that exposes data via API endpoints
8. Test endpoints with Postman

**Checklist:**
- [ ] Prisma installed and initialized
- [ ] Database connection configured
- [ ] Schema created with at least one model
- [ ] Migration run successfully
- [ ] PrismaService created and integrated
- [ ] API endpoints return data from database
- [ ] Tested with Postman

**AI-Assisted Learning:**
- Use AI to generate your Prisma schema
- Ask AI to explain any Prisma query you don't understand
- Use AI to debug Prisma connection issues
- Have AI review your Prisma service setup

---

## Knowledge Checks

- Q: What is an ORM and why use one?
- Q: What does `@id @default(autoincrement())` do?
- Q: What's the difference between `findMany()` and `findUnique()`?
- Q: Why do we need migrations?

---

## Common Pitfalls

- **Database not running:** Make sure PostgreSQL is running
- **Wrong connection string:** Double-check your DATABASE_URL
- **Forgot to generate client:** Run `npx prisma generate` after schema changes
- **Migration conflicts:** Reset database if needed: `npx prisma migrate reset`

**AI-Assisted Learning:**
If you're stuck, ask your AI: "I'm having trouble with Prisma. [describe issue]. What should I check?"

---

## Homework (60-90 minutes)

### Exercise: Build a Simple Database API

1. Create a Prisma schema for a "Task" or "Todo" application:
   - Task model with: id, title, description, completed, createdAt
2. Run migrations
3. Create PrismaService
4. Create TaskService with CRUD operations
5. Create TaskController with endpoints:
   - GET /tasks (all tasks)
   - GET /tasks/:id (single task)
   - POST /tasks (create task)
   - PATCH /tasks/:id (update task)
   - DELETE /tasks/:id (delete task)
6. Test all endpoints with Postman

**Acceptance Criteria:**
- [ ] Prisma schema designed and migrated
- [ ] All CRUD operations implemented
- [ ] Endpoints tested and working
- [ ] Code organized with services and controllers

**AI-Assisted Learning:**
- Use AI to design your schema
- Ask AI to generate service methods
- Use AI to debug any issues
- Have AI review your code structure

---

## Troubleshooting

**Problem:** "Can't reach database server"
- **Solution:** Check if PostgreSQL is running and connection string is correct

**Problem:** "Migration failed"
- **Solution:** Check your schema syntax and database permissions

**Problem:** "PrismaClient is not generated"
- **Solution:** Run `npx prisma generate`

**AI-Assisted Learning:**
Ask your AI: "My Prisma setup isn't working. [describe issue]. What steps should I take to debug this?"

---

## Navigation

- Back: ../learning-plan.md
- Week 03 Index: ../week-03.md
- Prev: ../../week-02/session-02.md
- Next: ../../week-04/session-04.md

