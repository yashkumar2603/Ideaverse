You might think that `mongoose` does add strictness to the codebase because we used to define a schema there. That strictness is present at the Node.js level, not at the DB level. You can still put in erroneous data in the database that doesn’t follow that schema.
Mongoose enforces the API side strictness at node.js level. But it is not present in the DB level, we can push anything from the mongodb compass.
But SQL databses dont let us do this at all.
##### start a Potgres database in a few ways -
- Using NeonDB
- Using Docker locally `docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -d -p 5432:5432 postgres`, connection string: `postgresql://postgres:mysecretpassword@localhost:5432/postgres?sslmode=disable`.
### psql
`psql` is a terminal-based front-end to PostgreSQL. It provides an interactive command-line interface to the PostgreSQL (or TimescaleDB) database. With psql, you can type in queries interactively, issue them to PostgreSQL, and see the query results.
**How to connect to your database?**
psql Comes bundled with postgresql. You don’t need it for this tutorial. We will directly be communicating with the database from Node.js
```javascript
psql -h p-broken-frost-69135494.us-east-2.aws.neon.tech -d database1 -U 100xdevs
```
### pg
`pg` is a `Node.js` library that you can use in your backend app to store data in the Postgres DB (similar to `mongoose`). We will be installing this eventually in our app.

---- 
Until now, we have a database that we can interact with. The next step in case of postgres is to define the `schema` of your tables.

SQL stands for `Structured query language`. It is a language in which you can describe what/how you want to put data in the database.

To create a table, the command to run is -

```javascript
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

There are a few parts of this SQL statement, let’s decode them one by one
`**SERIAL**`: A PostgreSQL-specific data type for creating an auto-incrementing integer. Every time a new row is inserted, this value automatically increments, ensuring each user has a unique `**id**`.

--- 
In the end, postgres exposes a protocol that someone needs to talk to be able to send these commands (update, delete) to the database.
`psql` is one such library that takes commands from your terminal and sends it over to the database.
To do the same in a Node.js , you can use one of many `Postgres clients`
### **pg library**

[https://www.npmjs.com/package/pg](https://www.npmjs.com/package/pg)
Non-blocking PostgreSQL client for Node.js.
Documentation - [https://node-postgres.com/](https://node-postgres.com/)

**Connecting -**
```javascript
import { Client } from 'pg'
 
const client = new Client({
  host: 'my.database-server.com',
  port: 5334,
  database: 'database-name',
  user: 'database-user',
  password: 'secretpassword!!',
})

client.connect()
```

**Querying -**
```javascript
const result = await client.query('SELECT * FROM USERS;')
console.log(result)
```

```javascript
// write a function to create a users table in your database.
import { Client } from 'pg'
 
const client = new Client({
  connectionString: "postgresql://postgres:mysecretpassword@localhost/postgres"
})

async function createUsersTable() {
    await client.connect()
    const result = await client.query(`
        CREATE TABLE users (
            id SERIAL PRIMARY KEY,
            username VARCHAR(50) UNIQUE NOT NULL,
            email VARCHAR(255) UNIQUE NOT NULL,
            password VARCHAR(255) NOT NULL,
            created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
        );
    `)
    console.log(result)
}

createUsersTable();
```

### Transcations -
If our schema was something like - 
```
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE addresses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL,
    city VARCHAR(100) NOT NULL,
    country VARCHAR(100) NOT NULL,
    street VARCHAR(255) NOT NULL,
    pincode VARCHAR(20),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```
This is called a `relationship` , which means that the `Address` table is related to the `Users` table.
When defining the table, you need to define the `relationship`
Good question to have at this point is what queries are run when the user signs up and sends both their information and their address in a single request. Do we send two SQL queries into the database? What if one of the queries (address query for example) fails? This would require `transactions` in SQL to ensure either both the user information and address goes in, or neither does

```
BEGIN; -- Start transaction

INSERT INTO users (username, email, password)
VALUES ('john_doe', 'john_doe1@example.com', 'securepassword123');

INSERT INTO addresses (user_id, city, country, street, pincode)
VALUES (currval('users_id_seq'), 'New York', 'USA', '123 Broadway St', '10001');

COMMIT;
```

## Joins -
[SQL databases](https://projects.100xdevs.com/tracks/YOSAherHkqWXhOdlE4yE/sql-11)

