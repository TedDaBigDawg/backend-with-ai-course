# Week 12 — Session 12: Environment Variables & Configuration

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 12 Index: ../week-12.md
- Prev: ../../week-11/session-11.md
- Next: ../../week-13/session-13.md

**Session length:** 120 minutes

## Pre-class Checklist

- Application working
- Understanding of security basics

## Learning Objectives

- Manage environment variables securely
- Use configuration modules in NestJS
- Handle different environments
- Secure sensitive data
- Follow security best practices

## Key Terms

- Environment variables, .env, configuration, secrets, ConfigModule, security

## You Will Build

- Environment configuration
- Secure secret management
- Environment-specific configs

---

## 1) Environment Variables (25 minutes)

### What are Environment Variables?

Variables that change based on environment (dev, staging, production).

### Using .env Files

```env
# .env
DATABASE_URL=postgresql://user:pass@localhost:5432/mydb
JWT_SECRET=your-secret-key
PORT=3000
NODE_ENV=development
```

### Installing dotenv

```bash
npm install @nestjs/config
```

**AI-Assisted Learning:**
Ask your AI: "Explain environment variables and best practices. Generate .env.example template."

---

## 2) ConfigModule (30 minutes)

### Setting Up ConfigModule

```typescript
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      envFilePath: '.env',
    }),
  ],
})
export class AppModule {}
```

### Using Config Service

```typescript
import { ConfigService } from '@nestjs/config';

@Injectable()
export class AppService {
  constructor(private configService: ConfigService) {}

  getDatabaseUrl() {
    return this.configService.get<string>('DATABASE_URL');
  }
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate ConfigModule setup for NestJS. Include validation and type safety."

---

## 3) Security Best Practices (25 minutes)

### Never Commit Secrets

- Add `.env` to `.gitignore`
- Use `.env.example` for documentation
- Use different secrets per environment

### Validating Configuration

```typescript
import { IsString, IsNumber } from 'class-validator';

export class EnvironmentVariables {
  @IsString()
  DATABASE_URL: string;

  @IsString()
  JWT_SECRET: string;

  @IsNumber()
  PORT: number;
}
```

**AI-Assisted Learning:**
Ask your AI: "Generate security best practices for managing secrets in NestJS. Include validation examples."

---

## 4) Guided Lab (20 minutes)

**Goal:** Configure your application with environment variables.

**Steps:**
1. Install ConfigModule
2. Create .env file
3. Configure ConfigModule
4. Use ConfigService
5. Test configuration

**Checklist:**
- [ ] ConfigModule configured
- [ ] Environment variables set
- [ ] ConfigService used
- [ ] Tested successfully

---

## Knowledge Checks

- Q: Why use environment variables?
- Q: How do you secure secrets?
- Q: What is ConfigModule used for?

---

## Common Pitfalls

- **Committing secrets:** Never commit .env files
- **Hardcoding values:** Use environment variables
- **No validation:** Validate configuration

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm configuring environment variables and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Configure Application

1. Set up ConfigModule
2. Move all secrets to .env
3. Add configuration validation
4. Test different environments

**Acceptance Criteria:**
- [ ] ConfigModule working
- [ ] Secrets in .env
- [ ] Validation added
- [ ] Tested successfully

---

## Navigation

- Back: ../learning-plan.md
- Week 12 Index: ../week-12.md
- Prev: ../../week-11/session-11.md
- Next: ../../week-13/session-13.md

