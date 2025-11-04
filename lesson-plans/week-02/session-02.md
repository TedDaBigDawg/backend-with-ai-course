# Week 02 — Session 02: HTTP, REST APIs & Database Introduction

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 02 Index: ../week-02.md
- Prev: ../../week-01/session-01.md
- Next: ../../week-03/session-03.md

**Session length:** 120 minutes

## Pre-class Checklist

- NestJS project from Week 01 ready
- Basic understanding of HTTP from Week 01
- Postman installed and ready

## Learning Objectives

- Understand RESTful API design principles
- Learn HTTP status codes and when to use them
- Grasp database fundamentals (what, why, types)
- Understand the relationship between APIs and databases
- Design API endpoints following REST conventions

## Key Terms

- REST, RESTful, endpoint, resource, HTTP status code, database, relational, NoSQL, schema, CRUD

## You Will Build

- Design REST API endpoints for a sample application
- Understand database concepts and types

---

## 1) REST API Design Principles (30 minutes)

### What is REST?

**REST (Representational State Transfer)** is an architectural style for designing web services.

**Key Principles:**
- **Stateless:** Each request contains all information needed
- **Resource-based:** URLs represent resources, not actions
- **HTTP methods:** Use GET, POST, PUT, DELETE appropriately
- **Standard responses:** Use HTTP status codes

### RESTful URL Design

**Good Examples:**
- `GET /users` - Get all users
- `GET /users/123` - Get user with ID 123
- `POST /users` - Create a new user
- `PUT /users/123` - Update user 123
- `DELETE /users/123` - Delete user 123

**Bad Examples:**
- `GET /getUsers` - Action in URL
- `POST /users/create` - Action in URL
- `GET /users/123/delete` - Wrong method

**AI-Assisted Learning:**
Ask your AI: "Generate examples of good vs bad REST API endpoint designs. Explain why each is good or bad."

---

## 2) HTTP Status Codes Deep Dive (20 minutes)

### Understanding Status Codes

Status codes tell the client what happened with their request.

### Common Status Codes

**2xx Success:**
- **200 OK:** Request succeeded
- **201 Created:** Resource created successfully
- **204 No Content:** Success but no content to return

**4xx Client Errors:**
- **400 Bad Request:** Invalid request data
- **401 Unauthorized:** Authentication required
- **403 Forbidden:** Not allowed to access
- **404 Not Found:** Resource doesn't exist
- **422 Unprocessable Entity:** Validation failed

**5xx Server Errors:**
- **500 Internal Server Error:** Server error
- **502 Bad Gateway:** Gateway error
- **503 Service Unavailable:** Service temporarily unavailable

### When to Use Each

**AI-Assisted Learning:**
Ask your AI: "Give me 10 API scenarios and tell me which HTTP status code to return for each."

---

## 3) Database Fundamentals (25 minutes)

### What is a Database?

A **database** stores data permanently so it can be retrieved later.

**Why We Need Databases:**
- **Persistence:** Data survives application restarts
- **Organization:** Structure data efficiently
- **Relationships:** Connect related data
- **Performance:** Fast queries and retrieval

### Database Types

**Relational Databases (SQL):**
- PostgreSQL, MySQL, SQL Server
- Data in tables with rows and columns
- Relationships between tables
- Uses SQL language

**NoSQL Databases:**
- MongoDB (documents)
- Redis (key-value)
- Different structures for different needs

**We're using PostgreSQL (Relational)**

### Database Concepts

**Tables:** Containers for data (like spreadsheets)
**Rows:** Individual records
**Columns:** Data fields
**Schema:** Structure/design of database
**Relations:** Connections between tables

**AI-Assisted Learning:**
Ask your AI: "Explain databases using analogies. What's the difference between SQL and NoSQL? When would you use each?"

---

## 4) APIs and Databases (20 minutes)

### How They Work Together

1. **Client** makes HTTP request to **API**
2. **API** receives request
3. **API** queries **Database** for data
4. **Database** returns data to **API**
5. **API** formats data and sends to **Client**

### Example Flow

**Request:** `GET /users/123`

1. Controller receives request
2. Service calls database to find user 123
3. Database returns user data
4. Service formats response
5. Controller returns JSON to client

**AI-Assisted Learning:**
Ask your AI: "Diagram the flow of a POST /users request from client to database and back. Explain each step."

---

## 5) Guided Lab: Design REST API Endpoints (25 minutes)

**Goal:** Design REST API endpoints for a blog application.

### Requirements

Design endpoints for:
- Users (create, read, update, delete)
- Posts (create, read, update, delete)
- Comments (create, read, delete)

### Solution

**Users:**
- `GET /users` - List all users
- `GET /users/:id` - Get user by ID
- `POST /users` - Create user
- `PUT /users/:id` - Update user
- `DELETE /users/:id` - Delete user

**Posts:**
- `GET /posts` - List all posts
- `GET /posts/:id` - Get post by ID
- `POST /posts` - Create post
- `PUT /posts/:id` - Update post
- `DELETE /posts/:id` - Delete post

**Comments:**
- `GET /posts/:postId/comments` - Get comments for a post
- `POST /posts/:postId/comments` - Create comment
- `DELETE /comments/:id` - Delete comment

**Checklist:**
- [ ] Endpoints follow REST conventions
- [ ] HTTP methods used correctly
- [ ] Appropriate status codes identified
- [ ] Endpoints are resource-based

**AI-Assisted Learning:**
Ask your AI: "Review my API endpoint design: [paste design]. Suggest improvements and explain why."

---

## Knowledge Checks

- Q: What makes an API RESTful?
- Q: What HTTP status code would you return for a successful creation?
- Q: What's the difference between SQL and NoSQL databases?
- Q: How do APIs and databases work together?

---

## Common Pitfalls

- **Putting actions in URLs:** Use HTTP methods, not verbs in URLs
- **Wrong status codes:** Learn when to use each code
- **Not understanding databases:** Databases are essential for backend development

**AI-Assisted Learning:**
If confused, ask your AI: "I'm designing an API and I'm not sure if [endpoint] follows REST principles. Can you review it?"

---

## Homework (60-90 minutes)

### Exercise: Design API for Your Application

Design REST API endpoints for one of these applications:

1. **E-commerce Store:**
   - Products, Orders, Customers, Cart

2. **Task Manager:**
   - Tasks, Projects, Users, Teams

3. **Social Media:**
   - Posts, Users, Likes, Follows

**Requirements:**
- Design all endpoints following REST principles
- Document HTTP methods and status codes
- Sketch out database tables needed
- Create a simple API documentation

**Acceptance Criteria:**
- [ ] All endpoints follow REST conventions
- [ ] HTTP methods used correctly
- [ ] Status codes identified for each endpoint
- [ ] Database tables sketched out
- [ ] Documentation created

**AI-Assisted Learning:**
- Use AI to brainstorm endpoint designs
- Ask AI to review your REST API design
- Use AI to suggest database tables
- Have AI generate API documentation examples

---

## Troubleshooting

**Problem:** Not sure if endpoint design is RESTful
- **Solution:** Ask yourself: "Does the URL represent a resource, not an action?"

**Problem:** Confused about status codes
- **Solution:** Think about the result: success (2xx), client error (4xx), server error (5xx)

**AI-Assisted Learning:**
Ask your AI: "I'm designing an API for [application]. Help me design REST endpoints and explain the reasoning."

---

## Navigation

- Back: ../learning-plan.md
- Week 02 Index: ../week-02.md
- Prev: ../../week-01/session-01.md
- Next: ../../week-03/session-03.md

