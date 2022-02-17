The sample code in this directory demonstrates how to connect to CockroachDB with the [C++ libpqxx driver](https://github.com/jtv/libpqxx).

## Step 1. Install the libpq and libpqxx drivers

1. Install `libpq` on your machine. For example, on macOS:

    ~~~ shell
    brew install libpq
    ~~~

1. Install the libpqxx driver, using [CMake](https://github.com/jtv/libpqxx/blob/master/BUILDING-cmake.md) or the [configure script](https://github.com/jtv/libpqxx/blob/master/configure) provided in the [`libpqxx` repo](https://github.com/jtv/libpqxx).

## Step 2. Create a database and a user


1. In the SQL shell, create the `bank` database that your application will use:

    ~~~ sql
    > CREATE DATABASE bank;
    ~~~

1. Create a SQL user for your app:

    ~~~ sql
    > CREATE USER <username> WITH PASSWORD <password>;
    ~~~

    Take note of the username and password. You will use it in your application code later.

1. Give the user the necessary permissions:

    ~~~ sql
    > GRANT ALL ON DATABASE bank TO <username>;
    ~~~

## Step 3. Run the code

### Basic statements

Run `basic-sample.cpp` to connect as the user you created earlier and execute some basic SQL statements, creating a table, inserting rows, and reading and printing the rows.

You might need to open `basic-sample.cpp`, and edit the connection configuration parameters to connect to your cluster.

To build the `basic-sample.cpp` source code to an executable file named `basic-sample`, run the following command:

~~~ shell
$ g++ -std=c++17 basic-sample.cpp -lpq -lpqxx -o basic-sample
~~~

Then run the `basic-sample` file from that directory:

~~~ shell
$ ./basic-sample
~~~

### Transaction (with retry logic)

Run `txn-sample.cpp` to again connect as the user you created earlier but this time execute a batch of statements as an atomic transaction to transfer funds from one account to another, where all included statements are either committed or aborted.

You might need to open `txn-sample.cpp`, and edit the connection configuration parameters to connect to your cluster.

To build the `txn-sample.cpp` source code to an executable file named `txn-sample`, run the following command:

~~~ shell
$ g++ -std=c++17 txn-sample.cpp -lpq -lpqxx -o txn-sample
~~~

Then run the `txn-sample` file from that directory:

~~~ shell
$ ./txn-sample
~~~