# Week 04 — Session 04: CRUD Operations & API Endpoints

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 04 Index: ../week-04.md
- Prev: ../../week-03/session-03.md
- Next: ../../week-05/session-05.md

**Session length:** 120 minutes

## Pre-class Checklist

- Prisma set up from Week 03
- Database schema created
- Understanding of NestJS controllers and services

## Learning Objectives

- Build complete CRUD operations with NestJS and Prisma
- Structure NestJS applications properly (controllers, services, modules)
- Handle HTTP requests and responses correctly
- Implement proper error handling
- Test APIs with Postman

## Key Terms

- CRUD, controller, service, module, dependency injection, error handling, exception

## You Will Build

- A complete CRUD API for a resource (e.g., Tasks, Products, Posts)

---

## 1) NestJS Architecture Review (15 minutes)

### Module Structure

```
FeatureModule
├── Controller (handles HTTP)
├── Service (business logic)
└── DTOs (data validation)
```

### Creating a Feature Module

```typescript
// tasks.module.ts
import { Module } from '@nestjs/common';
import { TasksController } from './tasks.controller';
import { TasksService } from './tasks.service';
import { PrismaService } from '../prisma.service';

@Module({
  controllers: [TasksController],
  providers: [TasksService, PrismaService],
})
export class TasksModule {}
```

**AI-Assisted Learning:**
Ask your AI: "Generate a complete NestJS module structure for a [feature] with controller, service, and module. Explain each part."

---

## 2) Building CRUD Operations (60 minutes)

### Step 1: Create Service

```typescript
// tasks.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { PrismaService } from '../prisma.service';

@Injectable()
export class TasksService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    return this.prisma.task.findMany();
  }

  async findOne(id: number) {
    const task = await this.prisma.task.findUnique({
      where: { id },
    });
    
    if (!task) {
      throw new NotFoundException(`Task with ID ${id} not found`);
    }
    
    return task;
  }

  async create(data: { title: string; description?: string }) {
    return this.prisma.task.create({
      data,
    });
  }

  async update(id: number, data: { title?: string; description?: string }) {
    await this.findOne(id); // Check if exists
    
    return this.prisma.task.update({
      where: { id },
      data,
    });
  }

  async remove(id: number) {
    await this.findOne(id); // Check if exists
    
    return this.prisma.task.delete({
      where: { id },
    });
  }
}
```

### Step 2: Create Controller

```typescript
// tasks.controller.ts
import { Controller, Get, Post, Body, Param, Patch, Delete } from '@nestjs/common';
import { TasksService } from './tasks.service';

@Controller('tasks')
export class TasksController {
  constructor(private readonly tasksService: TasksService) {}

  @Get()
  findAll() {
    return this.tasksService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.tasksService.findOne(+id);
  }

  @Post()
  create(@Body() createTaskDto: { title: string; description?: string }) {
    return this.tasksService.create(createTaskDto);
  }

  @Patch(':id')
  update(
    @Param('id') id: string,
    @Body() updateTaskDto: { title?: string; description?: string },
  ) {
    return this.tasksService.update(+id, updateTaskDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.tasksService.remove(+id);
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate a complete CRUD service and controller for [resource]. Include error handling and explain each method."

---

## 3) Error Handling (20 minutes)

### NestJS Exception Filters

NestJS provides built-in exceptions:

```typescript
import { NotFoundException, BadRequestException } from '@nestjs/common';

// Throw when resource not found
throw new NotFoundException('Task not found');

// Throw for bad requests
throw new BadRequestException('Invalid data');
```

### Custom Error Responses

```typescript
@Get(':id')
async findOne(@Param('id') id: string) {
  try {
    return await this.tasksService.findOne(+id);
  } catch (error) {
    if (error instanceof NotFoundException) {
      throw error; // Re-throw NestJS exceptions
    }
    throw new InternalServerErrorException('Something went wrong');
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "What are the best practices for error handling in NestJS? Generate examples of common error scenarios."

---

## 4) Guided Lab: Build Complete CRUD API (25 minutes)

**Goal:** Build a complete CRUD API for a resource.

**Steps:**
1. Create Prisma schema for your resource
2. Run migration
3. Create module, service, and controller
4. Implement all CRUD operations
5. Add error handling
6. Test with Postman

**Checklist:**
- [ ] Module created with controller and service
- [ ] All CRUD operations implemented (Create, Read, Update, Delete)
- [ ] Error handling added
- [ ] Tested with Postman
- [ ] Proper HTTP status codes returned

---

## Knowledge Checks

- Q: What's the difference between a controller and a service?
- Q: Which HTTP method would you use for updating a resource?
- Q: How do you handle "not found" errors in NestJS?

---

## Common Pitfalls

- **Forgetting dependency injection:** Services must be injected in constructors
- **Not handling errors:** Always check if resource exists before updating/deleting
- **Wrong HTTP methods:** Use PATCH for partial updates, PUT for full replacement

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm building CRUD operations and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Build Task Management API

Create a complete CRUD API for tasks with:
- GET /tasks (all tasks)
- GET /tasks/:id (single task)
- POST /tasks (create task)
- PATCH /tasks/:id (update task)
- DELETE /tasks/:id (delete task)

**Acceptance Criteria:**
- [ ] All CRUD operations working
- [ ] Proper error handling
- [ ] Tested with Postman
- [ ] Code well-organized

**Mini Project 1 Due:** Simple REST API with full CRUD operations

---

## Navigation

- Back: ../learning-plan.md
- Week 04 Index: ../week-04.md
- Prev: ../../week-03/session-03.md
- Next: ../../week-05/session-05.md

