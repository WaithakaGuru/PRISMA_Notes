# PRISMA ORM (Object-Relational Mapper)

# Table of Contents:
1. [Prisma Overview](#overview)
2. [Getting Started](#getting-started-with-prisma)
   - [Install Prisma](#installing-prisma)
   - [Set up Prisma in your Project](#setting-up-prisma-in-your-project)
3. [Models in Prisma](#models)
   - [Introduction to Models](#introduction-to-prisma-models)
   - [Field Types](#field-types)
   - [Field Attributes](#field-attributes)
   - [Field Modifiers](#field-modifiers)
   - [Create a Model](#creating-a-model)
4. [Migrations in Prisma](#migrations)
   - [Migrations Overview](#introduction-to-migrations)
   - [Perform a Migration](#performing-a-migration)
5. [The Prisma Client](#prisma-client)
   - [Overview](#overview-of-the-prisma-client)
   - [Create a Prisma Client](#creating-a-prisma-client)
6. [CRUD Operations in Prisma](#crud-operations-in-prisma)
   - [Create Operation](#create-operation)
   - [Read (select) Operation](#read-operation)
   - [Update Operation](#update-operation)
   - [Delete Operation](#delete-operation)
7. [Relationships in Prisma](#relationships)
   - [Overview](#introduction-to-relationships)
   - [One to One Relationship(1-1)](#one-to-one-relationship)
   - [One to Many Relationship(1-n)](#one-to-many-relationship)
   - [Many to Many relationship(k-n)](#many-to-many-relationship)
8. [Extra Features](#additonal-info)
   - [Dropping a Field in Prisma Model](#dropping-a-column-in-prisma)
   - [Dropping / deleting a Prisma Model](#dropping-a-modeltable-in-prisma)
   - [The Prisma Studio](#the-prisma-studio)
     - [Set up a Prisma Studio](#starting-the-prisma-studio)
     - [Benefits of Prisma Studio](#merits-of-the-prisma-studio)

---
 
# Overview

Prisma is a modern ORM (Object-Relational Mapper) for Node.js and TypeScript. It helps you work with databases in a type-safe and easy way, letting you write queries in JavaScript/TypeScript instead of SQL. Prisma supports popular databases like PostgreSQL, MySQL, SQLite, and more.

With Prisma, you define your data models in a schema file, and Prisma generates a client library for you to interact with your database. This makes database access safer and more productive.

---

# Getting Started with Prisma

  ## Installing Prisma

To get started, install Prisma and its CLI as development dependencies:

```bash
npm install prisma -D
```

  ## Setting up Prisma in your Project 

After installing, initialize Prisma in your project. This creates a `prisma` folder with a `schema.prisma` file:

```bash
npx prisma init
```

Edit the `schema.prisma` file to define your models.

It also creates a `.env` file in the root project folder from which you can edit the __database connection string__ commonly named as `DATABASE_URL=` fix the details of the string to match the connection details of your database.

---

# Models

  ## Introduction to Prisma Models 
  
Models in Prisma represent tables in your database. Each model is defined in the `schema.prisma` file and describes the fields (columns) and their types.

  ## Field types

Field types in Prisma include `String`, `Int`, `Boolean`, `DateTime`, `Byte`, `BigInt` and `Decimal`. These types map to the types supported by your database.

  ## Field attributes 
 
Attributes let you customize fields(columns) e.g: 
- making a field the primary key (`@id`)
- setting default values (`@default(default_value)`) 
- making a field unique (`@unique`).
  
## Field Modifiers 

Modifiers like `?` (optional) and `[]` (array) let you define if a field is required, optional or a list.

  ## Creating a model

 example of a `User` model:

```prisma
model User {
  id   Int  @id @default(autoincrement())
  name  String
  email String @unique
}
```
_Note_: ___Use `uuid()` to generate Unique and Random Ids.___

---

# Migrations 
  
  ## Introduction to Migrations

Migrations are used to update your database schema as your models change. Prisma generates migration files that describe the changes.

  ## Performing a Migration 

After applying changes to the schema.prisma file, run a migration with:

```bash
npx prisma migrate dev --name "Migration_name_to_describe_the_changes_made_"
```
This updates your database and keeps track of changes.

---

# Prisma Client 
 
 ## Overview of the Prisma Client 

The Prisma Client is an auto-generated library that lets you interact with your database using JavaScript or TypeScript. It provides type-safe queries and autocompletion.

 ## Creating a Prisma Client 

After defining your models and running a migration,
 __In the newer versions of Prisma the Client should be automatically generated__ but if the outo-generation fails,  

generate the client:

```bash
npx prisma generate
```

You can then use the client in your code:

```js
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();
```

---

# CRUD OPERATIONS in PRISMA

  ## Create Operation

Create a new record:

```js
const { PrismaClient } = require('@prisma/client');
const prisma = new PrismaClient();

async function createUser (){
 const newUser = await prisma.user.create({
  data: { name: 'Kevin', email: 'kevin@email.com' }
});
}
createUser();
```

  ## Read Operation 

Find records:

```js
const users = await prisma.user.findMany();
```

  ## Update Operation 

Update a record:

```js
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function updateUser() {
  const updatedUser = await prisma.user.update({
    where: { id: 1 },
    data: { name: 'Elian' }
  });
}
updateUser();
```

  ## Delete Operation 

Delete a record:

```js
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

async function deleteUser() {
  await prisma.user.delete({ where: { id: 1 } });
}
deleteUser();
```

---

# Relationships 

  ## Introduction to relationships

Relationships in Prisma let you connect models together, just like foreign keys in SQL. You can define one-to-one, one-to-many, and many-to-many relationships between your models. This makes it easy to represent real-world connections, like users and their posts, or students and their courses.

  ## One-to-One Relationship

A one-to-one relationship links a single record in one table to a single record in another. For example, each user can have one profile, and each profile belongs to one user.

__Prisma Schema Example__:

```prisma
model User {
  id       Int     @id @default(autoincrement())
  name     String
  profile  Profile?
}

model Profile {
  id       Int    @id @default(autoincrement())
  bio      String
  user     User   @relation(fields: [userId], references: [id])
  userId   Int    @unique
}
```

__Sample Data:__
- User: Jonteh
- Profile: { bio: "Full Stack Developer" }

__How to create a user with a profile:__

```js
const user = await prisma.user.create({
  data: {
    name: 'Jonteh',
    profile: {
      create: { bio: 'Full Stack Developer' }
    }
  },
  include: { profile: true }
});
```

---

  ## One-to-Many Relationship

A one-to-many relationship connects a single record in one table to multiple records in another. For example, a user can have many posts, but each post belongs to one user.

__Prisma Schema Example:__

```prisma
model User {
  id Int @id @default(autoincrement())
  name  String
  posts Post[]
}

model Post {
  id Int @id @default(autoincrement())
  title   String
  content String
  user User @relation(fields: [userId], references: [id])
  userId  Int
}
```

__Sample Data:__
- User: Piri
- Posts: ["Prisma Basics", "Advanced Prisma"]

__How to create a user with multiple posts:__

```js
const user = await prisma.user.create({
  data: {
    name: 'Piri',
    posts: {
      create: [
        { title: 'Prisma Basics', content: 'Intro to ORM' },
        { title: 'Advanced Prisma', content: 'Deep dive' }
      ]
    }
  },
  include: { posts: true }
});
```

---

  ## Many-to-Many Relationship 

A many-to-many relationship allows multiple records in one table to be related to multiple records in another. For example, students can enroll in many courses, and each course can have many students.

__Prisma Schema Example:__

```prisma
model Student {
  id Int @id @default(autoincrement())
  name String
  courses Course[]
}

model Course {
  id   Int  @id @default(autoincrement())
  title    String
  students Student[]
}
```

__Sample Data:__
- Students: Amos, Amon, Mc Garthy
- Courses: "Math", "Science"

__How to enroll students in courses:__

```js
import {PrismaClient} from '@prisma/client';

const prisma = new PrismaClient();
const course = await prisma.course.create({
  data: {
    title: 'Math',
    students: {
      create: [
        { student: { create: { name: 'Amos' } } },
        { student: { create: { name: 'Amon' } } }
      ]
    }
  },
  include: { students: true }
});

// Or add an existing student to a course
import {PrismaClient} from '@prisma/client';

const prisma = new PrismaClient();
await prisma.course.update({
  where: { id: 1 },
  data: {
    students: {
      connect: [{ id: 3 }] // Mc Garthy
    }
  }
});
```

---

# Additonal Info 

  ## Dropping a column in Prisma

To remove a field (column) from a model in Prisma, simply delete the field from your `schema.prisma` file. For example, if you want to remove the `bio` field from the `Profile` model:

```prisma
model Profile {
  id     Int  @id @default(autoincrement())
  // bio   String   // comment or delete this line
  user   User @relation(fields: [userId], references: [id])
  userId Int  @unique
}
```

After editing the schema, run a migration to update your database:

```bash
npx prisma migrate dev --name "remove-bio-field"
```

Prisma will generate a migration that drops the column from your database table.

---

  ## Dropping a model(table) in Prisma

To delete a table, remove the entire model from your `schema.prisma` file. For example, to drop the `Profile` table, delete the whole model:

```prisma
// Remove this model
delete model Profile {
  id     Int  @id @default(autoincrement())
  user   User @relation(fields: [userId], references: [id])
  userId Int  @unique
}
```

Then run a migration to apply the change:

```bash
npx prisma migrate dev --name drop-profile-table
```

This will remove the table from your database.

---

  ## The Prisma Studio 

Prisma Studio is a visual editor for your database. It lets you view and edit data in your tables through a simple web interface.

   ###  Starting the Prisma Studio

To open Prisma Studio, run:

```bash
npx prisma studio
```

This will launch a local web app where you can browse, add, edit, and delete records in your database easily.

---

   ###  Merits of the Prisma Studio

- __User-friendly interface__: Makes it easy to view and manage your data.
- __Live editing__: Add, update, or delete records directly from your browser.
- __Safe__: Works with your Prisma schema, so you only see and edit valid tables and fields.
- __Great for development__: Quickly test and debug your application data without writing SQL.
