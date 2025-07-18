# Setting up Prisma with Local PostgreSQL in Next.js

To set up Prisma with your own local PostgreSQL database in a Next.js project, follow these steps:

## 1. Install Dependencies

In your Next.js project directory, install Prisma and its dependencies:

```bash
npm install prisma --save-dev
npm install @prisma/client
```

[How to use Prisma ORM with Next.js](https://www.prisma.io/docs/guides/nextjs)

## 2. Initialize Prisma

Run the following command to initialize Prisma. This will create a `prisma` directory with a `schema.prisma` file and a `.env` file:

```bash
npx prisma init
```

[Connect your database using JavaScript and PostgreSQL](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-node-postgresql)

## 3. Configure the Database Connection

Edit the `.env` file and set your local PostgreSQL connection string. For example:

```env
DATABASE_URL="postgresql://YOUR_USER:YOUR_PASSWORD@localhost:5432/YOUR_DB?schema=public"
```

Replace `YOUR_USER`, `YOUR_PASSWORD`, and `YOUR_DB` with your actual PostgreSQL credentials and database name. The default schema is usually `public` [Connect your database using TypeScript and PostgreSQL](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/connect-your-database-typescript-postgresql).

## 4. Define Your Data Model

Edit `prisma/schema.prisma` to define your models. For example:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id         Int      @id             @default(autoincrement())
  name       String?
  email      String   @unique
  created_at DateTime @default(now())
}
```

[How to use Prisma ORM with Next.js](https://www.prisma.io/docs/guides/nextjs)

## 5. Run Migrations

Create your database tables based on your schema:

```bash
npx prisma migrate dev --name init
```

[How to use Prisma ORM with Next.js](https://www.prisma.io/docs/guides/nextjs)

## 6. Use Prisma Client in Your App

Create a `lib/prisma.ts` file to instantiate Prisma Client:

```typescript
import { PrismaClient } from '@/generated/prisma'

const globalForPrisma = global as unknown as { prisma: PrismaClient }

const prisma = globalForPrisma.prisma || new PrismaClient()

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma

export default prisma
```

[How to use Prisma ORM with Next.js – Set up Prisma Client](https://www.prisma.io/docs/guides/nextjs#25-set-up-prisma-client)

You can now import and use `prisma` in your Next.js API routes or server components.

---

**Note:** Make sure your local PostgreSQL server is running and accessible with the credentials you provided.

## 6. Update eslint.config.mjs file
```mjs
import { dirname } from "path";
import { fileURLToPath } from "url";
import { FlatCompat } from "@eslint/eslintrc";

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

const compat = new FlatCompat({
    baseDirectory: __dirname,
});

const eslintConfig = [
    {
        ignores: ["src/generated/prisma/**"],
    },
    ...compat.extends("next/core-web-vitals", "next/typescript"),
];

export default eslintConfig;
```

## 7. Models and CRUD operations
[Prisma Client models and CRUD operations](https://www.prisma.io/docs/orm/reference/prisma-client-reference)


## 8. Relations
[Prisma Relations](https://www.prisma.io/docs/orm/prisma-schema/data-model/relations)

## 9. Searching
[Prisma text-searching](https://www.prisma.io/docs/orm/prisma-client/queries/full-text-search)

## 10. Filtering and Sorting
[Prisma filtering-sorting](https://www.prisma.io/docs/orm/prisma-client/queries/filtering-and-sorting)

## 11. Pagination
[Prisma pagination](https://www.prisma.io/docs/orm/prisma-client/queries/pagination)

