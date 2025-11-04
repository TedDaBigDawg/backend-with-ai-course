# Week 06 — Session 06: Authentication Part I — JWT & Signup/Login

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 06 Index: ../week-06.md
- Prev: ../../week-05/session-05.md
- Next: ../../week-07/session-07.md

**Session length:** 120 minutes

## Pre-class Checklist

- CRUD operations working
- Understanding of HTTP and APIs
- Database with User model ready

## Learning Objectives

- Understand authentication vs authorization
- Learn how JWT tokens work
- Hash passwords securely using bcrypt
- Implement user registration endpoint
- Implement login endpoint with JWT generation

## Key Terms

- Authentication, authorization, JWT, token, password hashing, bcrypt, salt, signup, login

## You Will Build

- User registration endpoint
- Login endpoint with JWT
- Password hashing

---

## 1) Authentication Concepts (20 minutes)

### Authentication vs Authorization

**Authentication:** "Who are you?" (login, verify identity)
**Authorization:** "What can you do?" (permissions, roles)

### How Authentication Works

1. User provides credentials (email/password)
2. Server verifies credentials
3. Server generates a token (JWT)
4. Client stores token and sends it with requests
5. Server validates token on each request

**AI-Assisted Learning:**
Ask your AI: "Explain authentication flow with JWT step-by-step. Include a diagram description."

---

## 2) JWT Tokens (25 minutes)

### What is JWT?

**JWT (JSON Web Token)** is a compact way to securely transmit information.

**Structure:**
- Header (algorithm, type)
- Payload (data, user info)
- Signature (verification)

### Installing JWT Package

```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt
npm install -D @types/passport-jwt bcrypt
npm install bcrypt
```

**AI-Assisted Learning:**
Ask your AI: "Generate a JWT authentication setup for NestJS. Include signup and login endpoints."

---

## 3) Password Hashing (20 minutes)

### Why Hash Passwords?

Never store passwords in plain text! Always hash them.

### Using bcrypt

```typescript
import * as bcrypt from 'bcrypt';

// Hash password
const hashedPassword = await bcrypt.hash(password, 10);

// Verify password
const isValid = await bcrypt.compare(password, hashedPassword);
```

**AI-Assisted Learning:**
Ask your AI: "Explain password hashing and why it's important. Show bcrypt examples."

---

## 4) Implementing Signup (25 minutes)

### User Service

```typescript
import { Injectable, ConflictException } from '@nestjs/common';
import { PrismaService } from '../prisma.service';
import * as bcrypt from 'bcrypt';

@Injectable()
export class AuthService {
  constructor(private prisma: PrismaService) {}

  async signup(email: string, password: string, name?: string) {
    // Check if user exists
    const existing = await this.prisma.user.findUnique({
      where: { email },
    });
    
    if (existing) {
      throw new ConflictException('Email already exists');
    }
    
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Create user
    const user = await this.prisma.user.create({
      data: {
        email,
        password: hashedPassword,
        name,
      },
    });
    
    // Remove password from response
    const { password: _, ...result } = user;
    return result;
  }
}
```

### Signup Controller

```typescript
@Post('signup')
async signup(@Body() signupDto: { email: string; password: string; name?: string }) {
  return this.authService.signup(
    signupDto.email,
    signupDto.password,
    signupDto.name,
  );
}
```

**AI-Assisted Learning:**
Ask your AI: "Review my signup implementation. Suggest security improvements."

---

## 5) Implementing Login (30 minutes)

### Login with JWT

```typescript
import { JwtService } from '@nestjs/jwt';

@Injectable()
export class AuthService {
  constructor(
    private prisma: PrismaService,
    private jwtService: JwtService,
  ) {}

  async login(email: string, password: string) {
    // Find user
    const user = await this.prisma.user.findUnique({
      where: { email },
    });
    
    if (!user) {
      throw new UnauthorizedException('Invalid credentials');
    }
    
    // Verify password
    const isValid = await bcrypt.compare(password, user.password);
    
    if (!isValid) {
      throw new UnauthorizedException('Invalid credentials');
    }
    
    // Generate JWT
    const payload = { sub: user.id, email: user.email };
    const token = this.jwtService.sign(payload);
    
    return {
      access_token: token,
      user: {
        id: user.id,
        email: user.email,
        name: user.name,
      },
    };
  }
}
```

### Configure JWT Module

```typescript
// auth.module.ts
import { JwtModule } from '@nestjs/jwt';

@Module({
  imports: [
    JwtModule.register({
      secret: 'your-secret-key',
      signOptions: { expiresIn: '1d' },
    }),
  ],
  // ...
})
export class AuthModule {}
```

**AI-Assisted Learning:**
Ask your AI: "Generate a complete login implementation with JWT. Include error handling."

---

## 6) Guided Lab (20 minutes)

**Goal:** Implement signup and login endpoints.

**Steps:**
1. Install required packages
2. Update User model (add password field if needed)
3. Create AuthService with signup and login
4. Create AuthController
5. Test with Postman

**Checklist:**
- [ ] Passwords hashed with bcrypt
- [ ] Signup endpoint working
- [ ] Login endpoint returns JWT
- [ ] Error handling implemented
- [ ] Tested with Postman

---

## Knowledge Checks

- Q: Why hash passwords?
- Q: What does JWT stand for?
- Q: What's the difference between authentication and authorization?

---

## Common Pitfalls

- **Storing plain passwords:** Always hash!
- **Weak secrets:** Use strong JWT secrets
- **Not validating input:** Validate email and password

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm implementing authentication and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Complete Authentication

1. Implement signup with password hashing
2. Implement login with JWT generation
3. Add input validation
4. Test complete flow

**Acceptance Criteria:**
- [ ] Signup hashes passwords
- [ ] Login generates JWT
- [ ] Validation added
- [ ] Tested and working

---

## Navigation

- Back: ../learning-plan.md
- Week 06 Index: ../week-06.md
- Prev: ../../week-05/session-05.md
- Next: ../../week-07/session-07.md

