# Prisma

## Install

```
npm i -D prisma
```

## Initialise

```
npx prisma init --datasource-provider sqlite
```

## Formatting

Install Prisma extension.
Add to Settings.json

```json
  "[prisma]": {
    "editor.defaultFormatter": "Prisma.prisma"
  }
```

## Create Model

- Access `prisma/schema.prisma`

```ts
model ModelName {
    item type flag
}
```

- Type for dates is `DateTime`
- Flags:
  - @id
  - @default
  - @unique
  - @updatedAt
  - @ignore
  - @map
  - @relation
  - @db
- Models can be used as types.
- Import from `@prisma/client`

### Push model to DB

```
npx prisma db push
```

## Seed the DB

### Create Script in package.json

```json
  "prisma": {
    "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
  },
```

- May need to install ts-node

```
npm i ts-node
```

### Create seed file

- in prisma dir, create `seed.ts`
- write the seed script

```ts
import { PrismaClient } from '@prisma/client';

// hardcode data
const data = [/* seed data */];

const prisma = new PrismaClient();

async function main() {

    /* if getting data from an api, fetch it here */

    for (item of data) {
        const result = await prisma./*model*/.upsert({
            where: { id: item.id}, // set the id to the item id
            update: {},
            create: item,
        });
    }
}

main()
  .then(async () => await prisma.$disconnect())
  .catch(async (e) => {
    console.error(e);
    await prisma.$disconnect();
    process.exit(1);
  });
```

### Seed the DB

```
npx prisma db seed
```

## Create DB Client file

```
lib/db.ts
```

Add this code:

```ts
import { PrismaClient } from "@prisma/client";

const prismaClientSingleton = () => {
  return new PrismaClient();
};

type PrismaClientSingleton = ReturnType<typeof prismaClientSingleton>;

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClientSingleton | undefined;
};

const prisma = globalForPrisma.prisma ?? prismaClientSingleton();

export default prisma;

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = prisma;
```

## Access DB

### Get data from db

```ts
// get one record
const result = prisma./*model*/.findUnique({ where: { key: criteria }});

// get many records
const results = prisma./*model*/.findMany({ where: { key: criteria}});

// get all records
const results = prisma./*model*/.findMany({});
const results = prisma./*model*/.findMany({ where: { key: undefined }});

// get many records OR all records
const results = prisma./*model*/.findMany({ where: { key: criteria === 'all' ? undefined : criteria }});
```

### Sort Results

- To sort the results, we pass `orderBy` property into the query object.

```ts
const results = await prisma./*model*/.findMany({
    where: { key: criteria },
    orderBy: { date: 'asc' }
})
```

## Prisma Studio

```
npx prisma studio
```
