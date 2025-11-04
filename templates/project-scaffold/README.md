# NestJS Project Scaffold

A starter template for NestJS backend projects with Prisma and PostgreSQL.

## Structure

```
project-scaffold/
├── src/
│   ├── app.module.ts
│   ├── app.controller.ts
│   ├── app.service.ts
│   ├── main.ts
│   └── prisma/
│       └── prisma.service.ts
├── prisma/
│   └── schema.prisma
├── .env.example
├── package.json
└── README.md
```

## Setup

1. Install dependencies:
   ```bash
   npm install
   ```

2. Copy `.env.example` to `.env` and configure:
   ```bash
   cp .env.example .env
   ```

3. Update `.env` with your database connection string

4. Generate Prisma Client:
   ```bash
   npx prisma generate
   ```

5. Run migrations (if needed):
   ```bash
   npx prisma migrate dev
   ```

6. Start the application:
   ```bash
   npm run start:dev
   ```

## Features

- Basic NestJS structure
- Prisma service setup
- Environment configuration
- Example module structure

## Next Steps

- Add your own modules
- Create your database schema
- Implement your features
- Follow the course lesson plans

## AI-Assisted Learning

- Ask AI to explain the project structure
- Use AI to generate additional modules
- Have AI help with database schema design
- Use AI to debug setup issues

