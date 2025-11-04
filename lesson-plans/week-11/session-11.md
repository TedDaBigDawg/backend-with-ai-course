# Week 11 — Session 11: External APIs & HTTP Clients

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 11 Index: ../week-11.md
- Prev: ../../week-10/session-10.md
- Next: ../../week-12/session-12.md

**Session length:** 120 minutes

## Pre-class Checklist

- Basic API working
- Understanding of HTTP requests

## Learning Objectives

- Use HTTP clients to call external APIs
- Handle API responses and errors
- Integrate third-party services
- Implement retry logic
- Mock external APIs

## Key Terms

- HTTP client, external API, integration, retry logic, webhook, third-party service

## You Will Build

- Service that calls external APIs
- Error handling for external calls
- Integration with third-party services

---

## 1) HTTP Clients in NestJS (30 minutes)

### HttpModule

```typescript
import { HttpModule } from '@nestjs/axios';
import { HttpService } from '@nestjs/axios';

@Module({
  imports: [HttpModule],
  // ...
})
export class AppModule {}
```

### Making HTTP Requests

```typescript
import { Injectable } from '@nestjs/common';
import { HttpService } from '@nestjs/axios';
import { firstValueFrom } from 'rxjs';

@Injectable()
export class ExternalApiService {
  constructor(private httpService: HttpService) {}

  async fetchData() {
    const response = await firstValueFrom(
      this.httpService.get('https://api.example.com/data')
    );
    return response.data;
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate examples of calling external APIs with NestJS HttpModule. Include error handling."

---

## 2) Error Handling (25 minutes)

### Handling Errors

```typescript
async fetchData() {
  try {
    const response = await firstValueFrom(
      this.httpService.get('https://api.example.com/data')
    );
    return response.data;
  } catch (error) {
    if (error.response) {
      // API responded with error
      throw new HttpException(
        error.response.data.message,
        error.response.status,
      );
    }
    throw new HttpException('External API error', 500);
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate error handling patterns for external API calls. Include retry logic."

---

## 3) Guided Lab (35 minutes)

**Goal:** Integrate an external API.

**Steps:**
1. Install HttpModule
2. Create service for external API
3. Handle responses and errors
4. Integrate into your API
5. Test integration

**Checklist:**
- [ ] External API integrated
- [ ] Error handling implemented
- [ ] Tested successfully

---

## Knowledge Checks

- Q: How do you call external APIs in NestJS?
- Q: Why handle external API errors?
- Q: What is retry logic?

---

## Common Pitfalls

- **Not handling errors:** Always handle external API errors
- **Rate limiting:** Be aware of API limits
- **Timeout:** Set appropriate timeouts

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm integrating an external API and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Integrate External API

1. Choose an external API (weather, payment, etc.)
2. Create service to call it
3. Handle errors
4. Integrate into your API

**Acceptance Criteria:**
- [ ] External API integrated
- [ ] Error handling working
- [ ] Tested successfully

---

## Navigation

- Back: ../learning-plan.md
- Week 11 Index: ../week-11.md
- Prev: ../../week-10/session-10.md
- Next: ../../week-12/session-12.md

