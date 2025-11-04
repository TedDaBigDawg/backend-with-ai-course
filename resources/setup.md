# Environment Setup (Windows, macOS, Linux)

Goal: Get your machine ready for backend development with Node.js, NestJS, PostgreSQL, and development tools.

## 1) Install Node.js

- Download: [https://nodejs.org/](https://nodejs.org/) (choose LTS version)
- Verify installation in a terminal:
  ```bash
  node --version
  npm --version
  ```
- Should show v18 or higher for Node.js

## 2) Install PostgreSQL

### Option A: Local Installation

**Windows:**
- Download: [https://www.postgresql.org/download/windows/](https://www.postgresql.org/download/windows/)
- Run installer and remember your password
- PostgreSQL runs on port 5432 by default

**macOS:**
```bash
brew install postgresql
brew services start postgresql
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
```

### Option B: Cloud Database (Recommended for Beginners)

- **Neon:** [https://neon.tech/](https://neon.tech/) - Free tier available
- **Supabase:** [https://supabase.com/](https://supabase.com/) - Free tier available
- Create account and database
- Copy connection string (you'll use this in .env file)

## 3) Install Git

- Download: [https://git-scm.com/downloads](https://git-scm.com/downloads)
- Configure your identity:
  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```
- Create a GitHub account: [https://github.com/](https://github.com/)

## 4) Install VS Code

- Download: [https://code.visualstudio.com/](https://code.visualstudio.com/)
- Recommended extensions:
  - ESLint
  - Prettier
  - Prisma
  - GitLens
  - REST Client (for testing APIs)

## 5) Install NestJS CLI

```bash
npm install -g @nestjs/cli
```

Verify installation:
```bash
nest --version
```

## 6) Install Postman (Optional but Recommended)

- Download: [https://www.postman.com/downloads/](https://www.postman.com/downloads/)
- Alternative: Use VS Code REST Client extension or Insomnia

## 7) Create Your First Project

```bash
nest new my-backend-api
cd my-backend-api
npm run start:dev
```

Your API should be running at `http://localhost:3000`

## 8) Install Prisma

In your NestJS project:

```bash
npm install prisma @prisma/client
npm install -D prisma
npx prisma init
```

## 9) Configure Database Connection

Create `.env` file in your project root:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

For cloud databases, use the connection string provided by your provider.

## 10) AI Tools Setup (Optional)

- **ChatGPT:** [https://chat.openai.com/](https://chat.openai.com/)
- **Claude:** [https://claude.ai/](https://claude.ai/)
- **GitHub Copilot:** [https://github.com/features/copilot](https://github.com/features/copilot)

## Troubleshooting

**Problem:** "node: command not found"
- **Solution:** Ensure Node.js is installed and added to PATH. Restart terminal.

**Problem:** "nest: command not found"
- **Solution:** Install NestJS CLI globally: `npm install -g @nestjs/cli`

**Problem:** "Cannot connect to PostgreSQL"
- **Solution:** Ensure PostgreSQL is running. Check connection string in .env file.

**Problem:** "Port 3000 already in use"
- **Solution:** Change port in `main.ts` or stop the process using port 3000.

**Problem:** Prisma connection errors
- **Solution:** Verify DATABASE_URL in .env file. Test connection with `npx prisma db pull`.

## Verification Checklist

- [ ] Node.js installed (v18+)
- [ ] PostgreSQL installed or cloud database set up
- [ ] Git installed and configured
- [ ] VS Code installed with extensions
- [ ] NestJS CLI installed
- [ ] Postman installed (optional)
- [ ] First NestJS project created
- [ ] Prisma initialized
- [ ] Database connection configured

## Next Steps

- Complete Week 01 Session 01
- Follow the lesson plans in `lesson-plans/`
- Use AI tools to help with setup if needed

## Getting Help

If you encounter issues:
1. Check the troubleshooting section above
2. Search for error messages online
3. Ask AI tools to explain the error
4. Open a GitHub issue in the course repository
5. Ask during office hours

