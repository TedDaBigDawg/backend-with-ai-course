# Week 09 — Session 09: Swagger API Documentation

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 09 Index: ../week-09.md
- Prev: ../../week-08/session-08.md
- Next: ../../week-10/session-10.md

**Session length:** 120 minutes

## Pre-class Checklist

- API endpoints working
- DTOs created
- Understanding of API structure

## Learning Objectives

- Understand why API documentation matters
- Set up Swagger in NestJS
- Document API endpoints with decorators
- Generate interactive API documentation
- Test APIs using Swagger UI

## Key Terms

- Swagger, OpenAPI, API documentation, decorator, schema, response

## You Will Build

- Swagger setup
- Documented API endpoints
- Interactive API docs

---

## 1) Introduction to Swagger (15 minutes)

### What is Swagger?

Swagger (OpenAPI) is a tool for documenting REST APIs.

**Benefits:**
- Interactive API documentation
- Test endpoints directly
- Auto-generated from code
- Share with frontend developers

**AI-Assisted Learning:**
Ask your AI: "Explain Swagger/OpenAPI and why API documentation is important."

---

## 2) Setting Up Swagger (25 minutes)

### Install Package

```bash
npm install @nestjs/swagger swagger-ui-express
```

### Configure Swagger

```typescript
// main.ts
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';

const config = new DocumentBuilder()
  .setTitle('My API')
  .setDescription('API description')
  .setVersion('1.0')
  .addBearerAuth()
  .build();

const document = SwaggerModule.createDocument(app, config);
SwaggerModule.setup('api', app, document);
```

Access at: `http://localhost:3000/api`

**AI-Assisted Learning:**
Ask your AI: "Generate Swagger configuration for NestJS. Include authentication setup."

---

## 3) Documenting Endpoints (40 minutes)

### Basic Documentation

```typescript
import { ApiTags, ApiOperation, ApiResponse } from '@nestjs/swagger';

@ApiTags('tasks')
@Controller('tasks')
export class TasksController {
  @Get()
  @ApiOperation({ summary: 'Get all tasks' })
  @ApiResponse({ status: 200, description: 'Returns all tasks' })
  findAll() {
    return this.tasksService.findAll();
  }
}
```

### Documenting DTOs

```typescript
import { ApiProperty } from '@nestjs/swagger';

export class CreateTaskDto {
  @ApiProperty({ description: 'Task title', example: 'Complete project' })
  title: string;

  @ApiProperty({ description: 'Task description', required: false })
  description?: string;
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate Swagger documentation examples for [your endpoints]. Include all decorators."

---

## 4) Guided Lab (30 minutes)

**Goal:** Add Swagger documentation to your API.

**Steps:**
1. Install Swagger packages
2. Configure Swagger in main.ts
3. Document all endpoints
4. Document DTOs
5. Test in Swagger UI

**Checklist:**
- [ ] Swagger configured
- [ ] All endpoints documented
- [ ] DTOs documented
- [ ] Authentication documented
- [ ] Tested in Swagger UI

---

## Knowledge Checks

- Q: Why is API documentation important?
- Q: How do you add Swagger to NestJS?
- Q: What decorators document endpoints?

---

## Common Pitfalls

- **Incomplete docs:** Document all endpoints
- **Missing examples:** Add examples for clarity
- **Not updating:** Keep docs in sync with code

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm documenting my API with Swagger and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Document Your API

1. Set up Swagger
2. Document all endpoints
3. Document all DTOs
4. Add examples and descriptions
5. Share with team/frontend

**Acceptance Criteria:**
- [ ] Swagger working
- [ ] All endpoints documented
- [ ] Complete documentation

**Mini Project 3 Due:** API with comprehensive Swagger documentation

---

## Navigation

- Back: ../learning-plan.md
- Week 09 Index: ../week-09.md
- Prev: ../../week-08/session-08.md
- Next: ../../week-10/session-10.md

