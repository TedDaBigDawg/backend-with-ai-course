# Week 07 — Session 07: Authentication Part II — Guards & Roles

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 07 Index: ../week-07.md
- Prev: ../../week-06/session-06.md
- Next: ../../week-08/session-08.md

**Session length:** 120 minutes

## Pre-class Checklist

- JWT authentication working from Week 06
- Understanding of JWT tokens
- User model with roles (optional)

## Learning Objectives

- Understand authorization and access control
- Implement NestJS guards
- Protect routes with authentication
- Implement role-based access control
- Extract user information from JWT tokens

## Key Terms

- Guard, authorization, RBAC, role-based access, decorator, interceptor, strategy

## You Will Build

- Authentication guards
- Protected routes
- Role-based access control

---

## 1) Understanding Guards (20 minutes)

### What are Guards?

Guards determine if a request should be handled by a route handler.

**Types:**
- **Authentication Guard:** Is user logged in?
- **Authorization Guard:** Does user have permission?

### How Guards Work

1. Request comes in
2. Guard checks authentication/authorization
3. If valid, request proceeds
4. If invalid, request is rejected

**AI-Assisted Learning:**
Ask your AI: "Explain NestJS guards with examples. How do they work?"

---

## 2) Creating JWT Strategy (25 minutes)

### Install Passport JWT

```typescript
// jwt.strategy.ts
import { Injectable } from '@nestjs/common';
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: 'your-secret-key',
    });
  }

  async validate(payload: any) {
    return { userId: payload.sub, email: payload.email };
  }
}
```

### Configure in Module

```typescript
import { PassportModule } from '@nestjs/passport';
import { JwtStrategy } from './jwt.strategy';

@Module({
  imports: [PassportModule],
  providers: [JwtStrategy],
})
export class AuthModule {}
```

**AI-Assisted Learning:**
Ask your AI: "Generate a JWT strategy for NestJS. Explain each part of the configuration."

---

## 3) Creating Authentication Guard (20 minutes)

### Basic Guard

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}
```

### Using Guards

```typescript
@Controller('tasks')
@UseGuards(JwtAuthGuard)
export class TasksController {
  // All routes require authentication
}
```

### Per-Route Guard

```typescript
@Get()
@UseGuards(JwtAuthGuard)
findAll() {
  // This route requires authentication
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate examples of using guards in NestJS controllers. Show global and route-specific guards."

---

## 4) Getting Current User (15 minutes)

### Custom Decorator

```typescript
// current-user.decorator.ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const CurrentUser = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user;
  },
);
```

### Using in Controller

```typescript
@Get('profile')
@UseGuards(JwtAuthGuard)
getProfile(@CurrentUser() user: any) {
  return user;
}
```

**AI-Assisted Learning:**
Ask your AI: "Explain how to extract user information from JWT in NestJS. Generate a custom decorator example."

---

## 5) Role-Based Access Control (30 minutes)

### Add Role to User Model

```prisma
model User {
  id    Int    @id @default(autoincrement())
  email String @unique
  role  String @default("user") // "user" or "admin"
}
```

### Role Guard

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<string[]>(
      'roles',
      context.getHandler(),
    );
    
    if (!requiredRoles) {
      return true;
    }
    
    const { user } = context.switchToHttp().getRequest();
    return requiredRoles.includes(user.role);
  }
}
```

### Roles Decorator

```typescript
import { SetMetadata } from '@nestjs/common';

export const Roles = (...roles: string[]) => SetMetadata('roles', roles);
```

### Using Roles

```typescript
@Delete(':id')
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin')
remove(@Param('id') id: string) {
  return this.tasksService.remove(+id);
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate a complete RBAC implementation for NestJS. Include guards, decorators, and usage examples."

---

## 6) Guided Lab (20 minutes)

**Goal:** Implement guards and protect routes.

**Steps:**
1. Create JWT strategy
2. Create authentication guard
3. Protect routes
4. Add role-based access
5. Test with Postman

**Checklist:**
- [ ] JWT strategy created
- [ ] Guards implemented
- [ ] Routes protected
- [ ] Role-based access working
- [ ] Tested with Postman

---

## Knowledge Checks

- Q: What's the difference between authentication and authorization?
- Q: How do guards work in NestJS?
- Q: How do you implement role-based access?

---

## Common Pitfalls

- **Not validating token:** Ensure strategy validates tokens
- **Missing guards:** Don't forget to add guards
- **Wrong role checks:** Verify role assignment logic

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm implementing guards and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Protect Your API

1. Implement JWT guards
2. Protect all routes that need authentication
3. Add role-based access (admin vs user)
4. Test complete flow

**Acceptance Criteria:**
- [ ] Guards implemented
- [ ] Routes protected
- [ ] RBAC working
- [ ] Tested and working

**Mini Project 2 Due:** Authentication-enabled API with protected routes

---

## Navigation

- Back: ../learning-plan.md
- Week 07 Index: ../week-07.md
- Prev: ../../week-06/session-06.md
- Next: ../../week-08/session-08.md

