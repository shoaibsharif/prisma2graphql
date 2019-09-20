# Migration `watch-20190920125951`

This migration has been generated at 9/20/2019, 12:59:51 PM.
You can check out the [state of the datamodel](./datamodel.prisma) after the migration.

## Database Steps

```sql
CREATE TABLE "lift"."Post" (
  "author" TEXT    REFERENCES "User"(id) ON DELETE SET NULL,
  "content" TEXT    ,
  "createdAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  "id" TEXT NOT NULL   ,
  "published" BOOLEAN NOT NULL DEFAULT true  ,
  "title" TEXT NOT NULL DEFAULT ''  ,
  "updatedAt" DATE NOT NULL DEFAULT '1970-01-01 00:00:00'  ,
  PRIMARY KEY ("id")
);

CREATE TABLE "lift"."User" (
  "email" TEXT NOT NULL DEFAULT ''  ,
  "id" TEXT NOT NULL   ,
  "name" TEXT    ,
  "password" TEXT NOT NULL DEFAULT ''  ,
  PRIMARY KEY ("id")
);

CREATE UNIQUE INDEX "lift"."User.email" ON "User"("email")
```

## Changes

```diff
diff --git datamodel.mdl datamodel.mdl
migration ..watch-20190920125951
--- datamodel.dml
+++ datamodel.dml
@@ -1,0 +1,30 @@
+generator photon {
+  provider = "photonjs"
+}
+
+generator nexus_prisma {
+  provider = "nexus-prisma"
+}
+
+datasource db {
+  provider = "sqlite"
+  url      = "file:dev.db"
+}
+
+model Post {
+  id        String   @default(cuid()) @id
+  createdAt DateTime @default(now())
+  updatedAt DateTime @updatedAt
+  published Boolean  @default(true)
+  title     String
+  content   String?
+  author    User?
+}
+
+model User {
+  id       String  @default(cuid()) @id
+  email    String  @unique
+  password String
+  name     String?
+  posts    Post[]
+}
```

## Photon Usage

You can use a specific Photon built for this migration (watch-20190920125951)
in your `before` or `after` migration script like this:

```ts
import Photon from '@generated/photon/watch-20190920125951'

const photon = new Photon()

async function main() {
  const result = await photon.users()
  console.dir(result, { depth: null })
}

main()

```
