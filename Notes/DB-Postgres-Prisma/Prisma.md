Prisma is used to convert SQL Queries to easy to write fetch commands.
	for example ->
```JavaScript
consst query = 'SELECT * FROM users WHERE email = $1';
const result = await client.query(query, ["harkirat@gmail.com"]);
//if we dont use prisma
```
instead of this, we can simply write -> 
```JavaScript
User.find({
	email: "harkirat@gmail.com"
})
//If we use prisma
//no matter what the DB is, it will convert this to the appropriate command and then send it to the DB and get back the response.
```
![[Pasted image 20250116230436.png]]Doesn't matter what DB we are using, Prisma will handle all the shit for us, we just need to write the command to prisma, it will send the appropriate query.
### Automatic migrations
In case of a simple Postgres app, it’s very hard to keep track of all the commands that were ran that led to the current schema of the table.
![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F085e8ad8-528e-47d7-8922-a23dc4016453%2F50a8ab5a-364d-4375-9ea8-64d347d9515f%2FScreenshot_2024-02-03_at_7.01.25_PM.png?table=block&id=e4ade148-bcf9-46f9-bd65-df5008e09dcb&cache=v2)

```javascript
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE NOT NULL
);

ALTER TABLE users
ADD COLUMN phone_number VARCHAR(15);
```
As your app grows, you will have a lot of these `CREATE` and `ALTER` commands.
ORMs (or more specifically Prisma) maintains all of these for you.
For example - [https://github.com/code100x/cms/tree/main/prisma/migrations](https://github.com/code100x/cms/tree/main/prisma/migrations)

### Steps to use - 
![[Pasted image 20250116230622.png]]
Prisma lets you chose between a few databases (MySQL, Postgres, Mongo)
You can update `prisma/schema.prisma` to setup what database you want to use.
![[Pasted image 20250116230653.png]]

Get the prisma extension from VSCode to aid u in writing code.
Prisma expects you to define the shape of your data in the `schema.prisma` file.
If your final app will have a Users table, it would look like this in the `schema.prisma` file
```javascript
model User {
  id         Int      @id @default(autoincrement())
  username   String   @unique
  password   String
  firstName  String
  lastName   String
}
```

### Generate migrations
You have created a single schema file. You haven’t yet run the `CREATE TABLE` commands. To run those and create `migration files` , run
```javascript
npx prisma migrate dev --name Initialize the schema
```
Your DB should now have the updated schema.
![[Pasted image 20250116230933.png]]
# Generating the prisma client
Client represents all the functions that convert
```javascript
User.create({email: "harkirat@gmail.com"})
```
into
```javascript
INSERT INTO users VALUES ...
```
Once you’ve created the `prisma/schema.prisma` , you can generate these `clients` that you can use in your Node.js app
### How to Generate the client
```javascript
npx prisma generate
```
This generates a new `client` for you.

## Client side operations -

#### Insert - 
function that let’s you insert data in the `Users` table.
```javascript
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

async function insertUser(username: string, password: string, firstName: string, lastName: string) {
  const res = await prisma.user.create({
    data: {
        username,
        password,
        firstName,
        lastName
    }
  })
  console.log(res);
}
insertUser("admin1", "123456", "harkirat", "singh")
```

#### Update -
function that let’s you update data in the `Users` table.
```javascript
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

interface UpdateParams {
    firstName: string;
    lastName: string;
}
async function updateUser(username: string, {
    firstName,
    lastName
}: UpdateParams) {
  const res = await prisma.user.update({
    where: { username },
    data: {
      firstName,
      lastName
    }
  });
  console.log(res);
}
updateUser("admin1", {
    firstName: "new name",
    lastName: "singh"
})
```

#### Get User Details - 
function that let’s you fetch the details of a user given their email
```javascript
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

async function getUser(username: string) {
  const user = await prisma.user.findFirst({
    where: {
        username: username
    }
  })
  console.log(user);
}

getUser("admin1");
```

Prisma let’s you define `relationships` to relate tables with each other.
##### Types of relationships
1. One to One
2. One to Many
3. Many to One
4. Many to Many

![[Pasted image 20250116233149.png]]
#### Relationship Explanation
The highlighted portions in the image show a **bidirectional relation** between the User and Todo models. Let's break down both parts:**In the User model:**
```
todos Todo[]
```
This field represents a one-to-many relationship where one user can have multiple todos. The `[]` syntax indicates that this is an array of Todo objects
**In the Todo model:**
```
user User @relation(fields: [userId], references: [id])
```
This defines the relationship configuration with two key components:
- `fields: [userId]` specifies which field in the Todo model stores the foreign key
- `references: [id]` indicates that the foreign key references the `id` field in the User mode

