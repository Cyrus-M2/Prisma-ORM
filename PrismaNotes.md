# Prisma ORM

An ORM (Object-Relational Mapping) is a technique or a tool that allows developers to interact with a database using an object-oriented approach, rather than writing SQL queries.

Some examples of ORMs include:

- SQLAlchemy (Python)
- Sequelize (JavaScript)
- Hibernate (Java)
- Prisma
- Drizzle

## Prisma

Prisma is a modern ORM (Object-Relational Mapper) for Node.js and TypeScript.

### Steps to use Prisma

1. Install Prisma as a dev dependency.
2. Set up Prisma project by creating a schema file.
3. Create your model(s).
4. Run migrations to reflect changes in your database.
5. Generate the client and use it in your project.

---

## Install Prisma

```bash
npm install prisma -D
# or
npm install prisma --save-dev

Set Up Prisma

npx prisma init --datasource-provider DATABASE

Replace DATABASE with the database that you are using. Assuming you are using PostgreSQL:

npx prisma init --datasource-provider postgresql

If you are using PostgreSQL, simply run:

npx prisma init

Function of npx prisma init

    Creates a prisma folder with schema.prisma (contains connection for your DB)

    Creates .env used for defining the environment variables

.env comes with the below URL for easy configuration:

DATABASE_URL="postgresql://johndoe:randompassword@localhost:5432/mydb?schema=public"

Models

Models represent the different tables in your database.

Models are made up of fields (fields correspond to the columns in your database).
Things to Note in Prisma Models

    Data Models: Define the tables and their columns (fields) in your database.

    Field Types: Specify the data types for each field (e.g., String, Int, Boolean).

    Relations: Describe how models relate to each other using relation fields.

    Attributes: Add additional metadata to fields and models using attributes like @id, @default, @relation.

Fields are made up of:

    Field name – required

    Data type of the field – required (e.g., String, Int)

    Field type modifier – not necessary

    Attributes – not necessary

NB: Field Modifiers are used to indicate only two things:

    Whether a field is optional (use ?)

    Whether a field can contain multiple values (use [])

Field Types

    String – text data

    Int – numbers (32-bit)

    BigInt – numbers (64-bit)

    Float – fractions

    Boolean – true or false

    DateTime

Field Attributes

    @id: primary key of the model.

    @default: sets a default value for the field.

    @unique: ensures the field has unique values across all rows in a table.

    @updatedAt: automatically updates a field to the current timestamp.

    @map: maps the field to a column with a different name in the database.

    @relation: defines relationships between models.

    @@id: a model attribute that defines a composite primary key using multiple fields.

    @@unique: a model attribute that ensures a unique constraint on a combination of multiple fields.

    @@map: a model attribute that maps the model to a table with a different name in the database.

Migrations

Migrations are a way to manage and apply changes to your database in a controlled and consistent way.
How to run migrations

npx prisma migrate dev --name MIGRATION-NAME

Prisma Client

Prisma Client is an auto-generated and type-safe database client that you use to interact with your database in a Node.js or TypeScript application.
Generate the client

npx prisma generate

If @prisma/client is not installed automatically, use:

npm install @prisma/client

CRUD Operations - Create
Creating a single record using create()

import { PrismaClient } from '@prisma/client';
const client = new PrismaClient();

const createUser = async () => {
    const newUser = await client.User.create({
        data: {
            userName: "Cyruson",
            ethnicity: "Kamba",
            age: 23
        }
    });

    console.log(newUser);
};

CRUD Operations - Read
Get all records using findMany()

import { PrismaClient } from "@prisma/client";
const client = new PrismaClient();

const getUser = async () => {
  const User = await client.User.findMany();
  console.log(User);
};

getUser();

Comparison Operators in Prisma

Example: Consider students' height in feet.

    equals: matches exactly
    { height: { equals: 4 } }

    not: not equal to
    { height: { not: 4 } }

    lt: less than
    { height: { lt: 5 } }

    lte: less than or equal to
    { height: { lte: 6 } }

    gt: greater than
    { height: { gt: 6 } }

    gte: greater than or equal to
    { height: { gte: 5 } }

    equals: matches exactly (case-sensitive)
    { name: { equals: "John" } }

    contains: string contains specified substring

    startsWith: string starts with specified string

    endsWith: string ends with specified string

    not: negates the condition

Relationships

How different models (tables) in a database are connected to each other.

    One-to-one relationship (1-1): one record in a table is associated with only one record in another table.

    One-to-many relationship (1-n): one record in a table can be associated with multiple records in another table.

    Many-to-many relationship (m-n): multiple records in one table can be associated with multiple records in another table.

One-to-one Relationship

model Employee {
  id          String       @id @default(uuid())
  name        String
  position    String
  workstation Workstation?

  @@map("employees")
}

model Workstation {
  computerId String   @unique
  location   String
  employeeId String   @unique
  employee   Employee @relation(fields: [employeeId], references: [id])

  @@map("workstations")
}

One-to-many Relationship

model Department {
  id        String     @id @default(uuid())
  name      String
  employees Employee[]

  @@map("departments")
}

model Employee {
  id           String     @id @default(uuid())
  name         String
  position     String
  departmentId String
  department   Department @relation(fields: [departmentId], references: [id])

  @@map("employees")
}