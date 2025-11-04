# Week 01 — Session 01: Backend Foundations & NestJS Setup

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 01 Index: ../week-01.md
- Next: ../../week-02/session-02.md

**Session length:** 120 minutes

## Pre-class Checklist

- Install Node.js LTS (v18 or higher) — see ../../resources/setup.md
- Install VS Code with recommended extensions
- Create a GitHub account
- Have access to an AI tool (ChatGPT, Claude, or GitHub Copilot)

## Learning Objectives

- Understand what backend development is and how it differs from frontend
- Grasp HTTP basics (request/response, methods, status codes)
- Set up a NestJS development environment
- Create your first NestJS application
- Use AI to explain concepts and generate starter code

## Key Terms

- Backend, frontend, API, HTTP, REST, server, client, request, response, endpoint, controller, route

## You Will Build

- A simple NestJS API with multiple routes that return JSON responses

---

## 1) What is Backend Development? (15 minutes)

### Understanding Frontend vs Backend

**Frontend:**
- What users see and interact with (browser, mobile app)
- Handles user interface and user experience
- Runs on the client's device

**Backend:**
- The "server-side" of applications
- Handles business logic, database operations, authentication
- Runs on a server
- Provides APIs that frontend applications consume

### The Client-Server Model

Think of it like a restaurant:
- **Client (Frontend):** The customer who orders food
- **Server (Backend):** The kitchen that prepares and serves the food
- **API:** The menu that defines what can be ordered

**AI-Assisted Learning:**
Ask your AI tool: "Explain the client-server model using a restaurant analogy. How does it relate to web applications?"

### HTTP Basics

**HTTP (HyperText Transfer Protocol):** The language clients and servers use to communicate.

**HTTP Methods:**
- **GET:** Retrieve data (like asking for a menu)
- **POST:** Create new data (like placing an order)
- **PUT:** Update existing data (like modifying an order)
- **DELETE:** Remove data (like canceling an order)

**HTTP Status Codes:**
- **200 OK:** Success
- **201 Created:** Successfully created
- **400 Bad Request:** Client error
- **404 Not Found:** Resource doesn't exist
- **500 Internal Server Error:** Server error

**AI-Assisted Learning:**
Ask your AI: "Quiz me on HTTP methods. Give me 5 scenarios and I'll tell you which method to use."

---

## 2) Introduction to NestJS (15 minutes)

### What is NestJS?

NestJS is a **progressive Node.js framework** for building efficient and scalable server-side applications.

**Why NestJS?**
- Built with TypeScript (type safety)
- Modular architecture (easy to organize code)
- Follows best practices and design patterns
- Great for building RESTful APIs
- Excellent documentation and community

### NestJS Architecture

**Key Concepts:**
- **Modules:** Organize code into features
- **Controllers:** Handle HTTP requests and responses
- **Services:** Contain business logic
- **Providers:** Can be injected into other classes

**AI-Assisted Learning:**
Ask your AI: "Generate a simple NestJS boilerplate app with a controller and explain each part line by line."

---

## 3) Setting Up Your Development Environment (20 minutes)

### Step 1: Install Node.js

1. Download Node.js LTS from [nodejs.org](https://nodejs.org/)
2. Verify installation:
   ```bash
   node --version
   npm --version
   ```

### Step 2: Install NestJS CLI

```bash
npm i -g @nestjs/cli
```

Verify installation:
```bash
nest --version
```

### Step 3: Create Your First NestJS Project

```bash
nest new my-first-api
cd my-first-api
npm run start:dev
```

Your API is now running at `http://localhost:3000`!

**AI-Assisted Learning:**
If you encounter errors during setup, ask your AI: "I'm getting this error: [paste error]. What does it mean and how do I fix it?"

---

## 4) Understanding Your First NestJS App (20 minutes)

### Project Structure

```
my-first-api/
├── src/
│   ├── app.controller.ts    # Handles routes
│   ├── app.service.ts        # Business logic
│   ├── app.module.ts         # Organizes everything
│   └── main.ts               # Entry point
├── package.json
└── tsconfig.json
```

### Key Files Explained

**main.ts:**
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
This starts the NestJS application on port 3000.

**app.module.ts:**
```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
This declares what controllers and services belong to this module.

**app.controller.ts:**
```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```
This defines a GET route at `/` that returns a string.

**AI-Assisted Learning:**
Ask your AI: "Explain the NestJS decorators (@Controller, @Get) and how they work."

---

## 5) Creating Your First Routes (30 minutes)

### Guided Lab: Build a Simple API

**Goal:** Create an API with multiple routes that return different JSON responses.

**Step 1:** Add a new route to `app.controller.ts`

```typescript
import { Controller, Get, Post } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }

  @Get('about')
  getAbout() {
    return {
      message: 'This is my first API',
      version: '1.0.0',
      author: 'Your Name'
    };
  }

  @Get('users')
  getUsers() {
    return [
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' }
    ];
  }
}
```

**Step 2:** Test your routes

- Visit `http://localhost:3000` → returns "Hello World!"
- Visit `http://localhost:3000/about` → returns JSON object
- Visit `http://localhost:3000/users` → returns array of users

**Step 3:** Add a POST route

```typescript
@Post('users')
createUser() {
  return {
    message: 'User created successfully',
    id: 3,
    timestamp: new Date().toISOString()
  };
}
```

**Checklist:**
- [ ] NestJS app is running
- [ ] Created at least 3 GET routes
- [ ] Created 1 POST route
- [ ] All routes return JSON data
- [ ] Tested all routes in browser or Postman

**AI-Assisted Learning:**
Ask your AI: "Generate a NestJS controller with routes for a blog (posts, comments, likes). Then explain what each decorator does."

---

## 6) Using Postman to Test APIs (10 minutes)

### What is Postman?

Postman is a tool for testing APIs. It's like a browser, but specifically for APIs.

### Testing Your Routes

1. Download Postman from [postman.com](https://www.postman.com/)
2. Create a new request
3. Set method to GET
4. Enter URL: `http://localhost:3000/about`
5. Click Send
6. See the JSON response

**Try different HTTP methods:**
- GET: `http://localhost:3000/users`
- POST: `http://localhost:3000/users` (method: POST)

---

## Knowledge Checks

- Q: What's the difference between frontend and backend?
- Q: What HTTP method would you use to retrieve a list of products?
- Q: What does `@Get()` decorator do in NestJS?
- Q: What port does NestJS run on by default?

---

## Common Pitfalls

- **Forgetting to save files:** NestJS watches for changes, but you need to save
- **Port already in use:** If port 3000 is busy, change it in `main.ts`
- **Not understanding decorators:** Decorators are like annotations that add functionality

**AI-Assisted Learning:**
If you're stuck, ask your AI: "I'm trying to create a route in NestJS but it's not working. Here's my code: [paste code]. What's wrong?"

---

## Homework (60-90 minutes)

### Exercise: Build a Simple Task API

Create a NestJS API with the following endpoints:

1. `GET /tasks` - Returns a list of tasks
2. `GET /tasks/:id` - Returns a single task by ID
3. `POST /tasks` - Creates a new task
4. `GET /health` - Returns API health status

**Acceptance Criteria:**
- [ ] NestJS project is set up and running
- [ ] All 4 endpoints are implemented
- [ ] Endpoints return appropriate JSON responses
- [ ] Code is organized with controllers and services
- [ ] Tested all endpoints with Postman

**AI-Assisted Learning:**
- Use AI to generate starter code for the task API
- Ask AI to explain any concepts you don't understand
- Use AI to debug errors you encounter
- Have AI review your code and suggest improvements

**Stretch Goals (Optional):**
- Add error handling for invalid task IDs
- Add validation for POST requests
- Create a simple frontend to display tasks

---

## Troubleshooting

**Problem:** "Cannot find module @nestjs/core"
- **Solution:** Run `npm install` in your project directory

**Problem:** Port 3000 is already in use
- **Solution:** Change port in `main.ts`: `await app.listen(3001);`

**Problem:** Routes not working
- **Solution:** Make sure you saved the file and NestJS restarted (watch for "Nest application successfully started")

**AI-Assisted Learning:**
When you encounter errors, ask your AI: "I'm getting this error: [error]. What does it mean and how do I fix it?"

---

## Navigation

- Back: ../learning-plan.md
- Next: ../../week-02/session-02.md

