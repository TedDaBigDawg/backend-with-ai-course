# Week 13 — Session 13: Deployment Basics & CI/CD Introduction

**Navigation**
- Back to Learning Plan: ../learning-plan.md
- Week 13 Index: ../week-13.md
- Prev: ../../week-12/session-12.md
- Next: ../../week-14/session-14.md

**Session length:** 120 minutes

## Pre-class Checklist

- Application complete
- Environment variables configured
- GitHub repository ready

## Learning Objectives

- Deploy NestJS applications to cloud platforms
- Understand CI/CD concepts
- Set up basic CI/CD pipelines
- Configure production environments
- Monitor deployed applications

## Key Terms

- Deployment, hosting, CI/CD, pipeline, production, environment, monitoring

## You Will Build

- Deployed application
- Basic CI/CD setup
- Production configuration

---

## 1) Deployment Platforms (30 minutes)

### Popular Platforms

- **Render:** Easy deployment, free tier
- **Railway:** Simple setup
- **Heroku:** Popular, easy to use
- **AWS/GCP/Azure:** More complex, more control

### Deployment Checklist

- [ ] Database configured
- [ ] Environment variables set
- [ ] Build script configured
- [ ] Start script configured

**AI-Assisted Learning:**
Ask your AI: "Compare deployment platforms for NestJS. Generate deployment guides for [platform]."

---

## 2) Deploying to Render (25 minutes)

### Steps

1. Create account on Render
2. Connect GitHub repository
3. Create new Web Service
4. Configure:
   - Build command: `npm install && npm run build`
   - Start command: `npm run start:prod`
5. Add environment variables
6. Deploy!

**AI-Assisted Learning:**
Ask your AI: "Generate step-by-step deployment guide for NestJS to Render. Include all configurations."

---

## 3) CI/CD Basics (25 minutes)

### What is CI/CD?

- **CI (Continuous Integration):** Automatically test code
- **CD (Continuous Deployment):** Automatically deploy code

### GitHub Actions Example

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      - run: npm install
      - run: npm test
      - run: npm run build
```

**AI-Assisted Learning:**
Ask your AI: "Generate GitHub Actions workflow for NestJS. Include testing and deployment steps."

---

## 4) Guided Lab (20 minutes)

**Goal:** Deploy your application.

**Steps:**
1. Choose deployment platform
2. Configure deployment
3. Set environment variables
4. Deploy application
5. Test deployed app

**Checklist:**
- [ ] Application deployed
- [ ] Environment variables set
- [ ] Database connected
- [ ] Application working

**Final Capstone Project Proposal Due:** Submit your project plan

---

## Knowledge Checks

- Q: What is CI/CD?
- Q: Why deploy to production?
- Q: What environment variables are needed?

---

## Common Pitfalls

- **Missing environment variables:** Set all required vars
- **Database connection:** Ensure database is accessible
- **Build errors:** Test build locally first

**AI-Assisted Learning:**
If stuck, ask your AI: "I'm deploying my app and [describe issue]. Help me fix it."

---

## Homework (60-90 minutes)

### Exercise: Deploy Your Application

1. Choose deployment platform
2. Deploy application
3. Configure production environment
4. Test deployed application

**Acceptance Criteria:**
- [ ] Application deployed
- [ ] Working in production
- [ ] Environment configured

---

## Navigation

- Back: ../learning-plan.md
- Week 13 Index: ../week-13.md
- Prev: ../../week-12/session-12.md
- Next: ../../week-14/session-14.md

