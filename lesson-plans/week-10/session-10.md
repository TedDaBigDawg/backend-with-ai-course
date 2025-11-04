# Week 10 — Session 10: Middleware & File Handling

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 10 Index: ../week-10.md
- Prev: ../../week-09/session-09.md
- Next: ../../week-11/session-11.md

**Session length:** 120 minutes

## Pre-class Checklist

- API with authentication working
- Understanding of NestJS architecture

## Learning Objectives

- Understand middleware and its uses
- Create custom middleware
- Implement file upload functionality
- Handle file validation and storage
- Serve static files

## Key Terms

- Middleware, file upload, multer, interceptor, file storage, static files

## You Will Build

- Custom middleware
- File upload endpoints
- File storage

---

## 1) Understanding Middleware (20 minutes)

### What is Middleware?

Middleware functions that run before route handlers.

**Common Uses:**
- Logging requests
- Parsing request data
- Authentication checks
- CORS handling

### Creating Middleware

```typescript
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`Request: ${req.method} ${req.url}`);
    next();
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Explain NestJS middleware with examples. Show logging and request transformation middleware."

---

## 2) File Uploads (40 minutes)

### Install Multer

```bash
npm install @nestjs/platform-express
npm install -D @types/multer
```

### File Upload Endpoint

```typescript
import { FileInterceptor } from '@nestjs/platform-express';
import { UseInterceptors, UploadedFile } from '@nestjs/common';

@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File) {
  return {
    filename: file.filename,
    size: file.size,
    mimetype: file.mimetype,
  };
}
```

### File Validation

```typescript
import { FileValidator } from '@nestjs/common';

export class FileSizeValidator extends FileValidator {
  constructor(protected readonly maxSize: number) {
    super({ maxSize });
  }

  isValid(file: Express.Multer.File): boolean {
    return file.size <= this.maxSize;
  }

  buildErrorMessage(): string {
    return `File size exceeds ${this.maxSize} bytes`;
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate file upload implementation for NestJS. Include validation and storage examples."

---

## 3) Guided Lab (30 minutes)

**Goal:** Implement file upload functionality.

**Steps:**
1. Set up file upload
2. Create upload endpoint
3. Add validation
4. Store files
5. Test with Postman

**Checklist:**
- [ ] File upload working
- [ ] Validation added
- [ ] Files stored
- [ ] Tested successfully

---

## Knowledge Checks

- Q: What is middleware used for?
- Q: How do you handle file uploads in NestJS?
- Q: Why validate uploaded files?

---

## Common Pitfalls

- **File size limits:** Set appropriate limits
- **Security:** Validate file types
- **Storage:** Choose storage location

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm implementing file uploads and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Add File Upload

1. Implement file upload endpoint
2. Add validation
3. Store files
4. Test upload functionality

**Acceptance Criteria:**
- [ ] File upload working
- [ ] Validation implemented
- [ ] Files stored correctly

---

## Navigation

- Back: ../learning-plan.md
- Week 10 Index: ../week-10.md
- Prev: ../../week-09/session-09.md
- Next: ../../week-11/session-11.md

