# Week 08 — Session 08: API Validation & Error Handling

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 08 Index: ../week-08.md
- Prev: ../../week-07/session-07.md
- Next: ../../week-09/session-09.md

**Session length:** 120 minutes

## Pre-class Checklist

- CRUD API working
- Understanding of controllers and services
- Basic error handling knowledge

## Learning Objectives

- Validate incoming request data
- Use DTOs for type safety and validation
- Implement consistent error handling
- Return appropriate HTTP status codes
- Create user-friendly error messages

## Key Terms

- DTO, validation, class-validator, exception filter, error handling, validation pipe

## You Will Build

- DTOs with validation
- Global error handling
- Custom error responses

---

## 1) Introduction to DTOs (20 minutes)

### What are DTOs?

**DTO (Data Transfer Object)** defines the structure of data sent to/from API.

### Creating DTOs

```typescript
// create-task.dto.ts
export class CreateTaskDto {
  title: string;
  description?: string;
  completed?: boolean;
}
```

### Using DTOs

```typescript
@Post()
create(@Body() createTaskDto: CreateTaskDto) {
  return this.tasksService.create(createTaskDto);
}
```

**AI-Assisted Learning:**
Ask your AI: "Explain DTOs in NestJS. Generate examples for different scenarios."

---

## 2) Validation with class-validator (30 minutes)

### Install Package

```bash
npm install class-validator class-transformer
```

### Enable Validation

```typescript
// main.ts
import { ValidationPipe } from '@nestjs/common';

app.useGlobalPipes(new ValidationPipe());
```

### Validation Decorators

```typescript
import { IsString, IsEmail, IsOptional, MinLength, MaxLength } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsString()
  @MinLength(6)
  @MaxLength(20)
  password: string;

  @IsString()
  @IsOptional()
  name?: string;
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate validation examples for [your use case]. Include all common validators."

---

## 3) Error Handling (30 minutes)

### Global Exception Filter

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    const status = exception instanceof HttpException
      ? exception.getStatus()
      : 500;

    const message = exception instanceof HttpException
      ? exception.getMessage()
      : 'Internal server error';

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      message,
    });
  }
}
```

### Using in main.ts

```typescript
app.useGlobalFilters(new AllExceptionsFilter());
```

**AI-Assisted Learning:**
Ask your AI: "Generate a comprehensive error handling setup for NestJS. Include custom exceptions."

---

## 4) Guided Lab (30 minutes)

**Goal:** Add validation and error handling to your API.

**Steps:**
1. Install class-validator
2. Create DTOs with validation
3. Enable global validation pipe
4. Implement error handling
5. Test validation errors

**Checklist:**
- [ ] DTOs created with validation
- [ ] Validation pipe enabled
- [ ] Error handling implemented
- [ ] User-friendly error messages
- [ ] Tested validation scenarios

---

## Knowledge Checks

- Q: What is a DTO?
- Q: Why validate input data?
- Q: How do you handle errors globally in NestJS?

---

## Common Pitfalls

- **Not validating:** Always validate input
- **Poor error messages:** Make errors helpful
- **Missing ValidationPipe:** Don't forget to enable it

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm implementing validation and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Add Validation

1. Create DTOs for all endpoints
2. Add validation decorators
3. Implement error handling
4. Test all validation scenarios

**Acceptance Criteria:**
- [ ] All DTOs validated
- [ ] Error handling working
- [ ] Tested scenarios

---

## Navigation

- Back: ../learning-plan.md
- Week 08 Index: ../week-08.md
- Prev: ../../week-07/session-07.md
- Next: ../../week-09/session-09.md

