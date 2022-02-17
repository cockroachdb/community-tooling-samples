The sample code in this directory demonstrates how to connect to CockroachDB using [leiningen](https://leiningen.org/) and the [Clojure java.jdbc driver](http://clojure-doc.org/articles/ecosystem/java_jdbc/home.html) in conjunction with the [PostgreSQL JDBC driver](https://jdbc.postgresql.org/)

## Step 1. Install `leiningen`

Install the Clojure `lein` utility as described in its [official documentation](https://leiningen.org/).

## Step 2. Create the `maxroach` user and `bank` database

Start the built-in SQL shell:

~~~ shell
$ cockroach sql --certs-dir=certs
~~~

In the SQL shell, issue the following statements to create the `maxroach` user and `bank` database:

~~~ sql
> CREATE USER IF NOT EXISTS maxroach;
~~~

~~~ sql
> CREATE DATABASE bank;
~~~

Give the `maxroach` user the necessary permissions:

~~~ sql
> GRANT ALL ON DATABASE bank TO maxroach;
~~~

Exit the SQL shell:

~~~ sql
> \q
~~~

## Step 3. Generate a certificate for the `maxroach` user

Create a certificate and key for the `maxroach` user by running the following command. The code samples will run as this user.

Pass the `--also-generate-pkcs8-key` flag to generate a key in [PKCS#8 format](https://tools.ietf.org/html/rfc5208), which is the standard key encoding format in Java. In this case, the generated PKCS8 key will be named `client.maxroach.key.pk8`.

~~~ shell
$ cockroach cert create-client maxroach --certs-dir=certs --ca-key=my-safe-directory/ca.key --also-generate-pkcs8-key
~~~

## Step 4. Create a table in the new database

As the `maxroach` user, use the built-in SQL client to create an `accounts` table in the new database.

~~~ shell
$ cockroach sql \
--certs-dir=certs \
--database=bank \
--user=maxroach \
-e 'CREATE TABLE accounts (id INT PRIMARY KEY, balance INT)'
~~~

## Step 5. Run the Clojure code

### Basic statements

Run `src/test/test.clj` to connect as the user you created earlier and execute some basic SQL statements, creating a table, inserting rows, and reading and printing the rows:

~~~ shell
$ lein run
~~~

You might need to open `test.clj`, and edit the connection configuration parameters to connect to your cluster.

### Transaction (with retry logic)

Run `src/test/test-txn.clj` to again connect as the user you created earlier but this time execute a batch of statements as an atomic transaction to transfer funds from one account to another, where all included statements are either committed or aborted.

You might need to open `test-txn.clj`, and edit the connection configuration parameters to connect to your cluster.
