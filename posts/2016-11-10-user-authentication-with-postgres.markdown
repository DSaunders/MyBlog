---
date   2016-11-10 17:00:00
---

We all know that we must salt and hash our passwords before we store them in the database.

Handily, Postgres can do this for you using the `pgcrypto` module. That means we don't need to worry about writing the code to do it ourselves, and we can create and authenticate users using simple SQL statements.

We'll be using the `psql` command line tool for this, but in a real app we'd be doing this in code.

# Getting started

We'll need somewhere to put our fake users, so let's create a database:

```sql
CREATE DATABASE blogtest;
\connect blogtest
```

Now, we need to add the `pgcrypto` module ..

```sql
CREATE EXTENSION pgcrypto;
```

.. and create a table to store our users in:

```sql
CREATE TABLE users (    
    name text NOT NULL,
    password text NOT NULL
);
```

Of course, in a real implementation our user would have an ID, and probably some other attributes, but this will do for testing.

# Adding new users

When we add new users to our table, we use the `crypt` function. The first parameter is the user's password. The second is the `gen_salt` function.
This generates the salt for our password, and also tells the `crypt` function which hashing algorithm to use.

Here, we use 8 iterations of the Blowfish (`'bf'`) algorithm:

```sql
INSERT INTO users (name, password) VALUES 
	('dave', crypt('password1', gen_salt('bf', 8)));
```

That's it! If we take a look at the `users` table, we can see our user has been added with a hashed password:

```sql
SELECT * FROM users;

 name |                           password                           
------+-------------------------------------------------------------
 dave | $2a$08$bJafH8Lh7ljQdMxFTtwZGuEKbWMjrZLqrdzbBAc7aXvBBltUq5.pS
(1 row)
```

# Authenticating users

Authenticating a user is just a simple `SELECT`.

We pass the password the user has entered into the `crypt` function, as well as the column containing the hashed password. If the password is correct, the row will be returned.

Let's try it with the correct password ('password1'):


```sql
SELECT * FROM users WHERE 
    name='dave' AND 
    password = crypt('password1', password);


 name |                           password                           
------+--------------------------------------------------------------
 dave | $2a$08$bJafH8Lh7ljQdMxFTtwZGuEKbWMjrZLqrdzbBAc7aXvBBltUq5.pS
(1 row)
```

Now, let's try the same thing, but this time we'll try to find the user using the incorrect password:

```sql
SELECT * FROM users WHERE 
    name='dave' AND 
    password = crypt('password99', password);


 name | password 
------+----------
(0 rows)
```

Authenticating a user is now a simple as checking whether a user is returned from this `SELECT` query.

# Conclusion

So, you can see that it's really easy to do hashing and user authentication using just Postgres and pgcrypto - by letting the database do all of the work for us.

Remember that we're passing the passwords to Postgres in plain text, so we need to make sure that communications between the app and the database are secure. That means we must be connecting locally, or using Postgres' native SSL support. 


If you want to find out more about pgcryto, check out:

<ul>
    <li><a href="https://www.postgresql.org/docs/8.3/static/pgcrypto.html" target="_blank">Documentation</a> for the pgcrypto module</li>
    <li>Securing your connection to Postgres <a href="https://www.postgresql.org/docs/9.1/static/ssl-tcp.html" target="_blank">with SSL</a></li>

    
</ul>

