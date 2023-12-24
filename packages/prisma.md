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

### Limit Results

- If we don't want every result to be returned, we can provide a maximum number of results to return.
- For this we provide the `take` property.

```ts
const results = await prisma./*model*/.findMany({
    where: { key: criteria },
    orderBy: { date: 'asc' },
    take: 10, // only get 10 results
})
```

### Skip Results

- For pagination purposes, we may want to skip results.
- For this we pass the `skip` property.

```ts
const results = await prisma./*model*/.findMany({
    where: { key: criteria },
    orderBy: { date: 'asc' },
    take: 10,
    skip: 20, // give me 10 results but skip the first 20.
})
```

- For pagination, using page numbers in query parameters we could do:

```ts
const resultLimit = 10;
// receive the page number from somewhere...
const results = await prisma./*model*/.findMany({
    where: { key: criteria },
    orderBy: { date: 'asc' },
    take: resultLimit,
    skip: (page - 1) * resultLimit;
})
```

### Number of Results

- We can do a slightly different request that returns the number of items that match the criteria.

```ts
const numResults = prisma./*model*/.count() // total in db
const numItems = prisma./*model*/.count({ where: { key: criteria }}) // number of matches where the key === the criteria
```

## Prisma Studio

```
npx prisma studio
```
