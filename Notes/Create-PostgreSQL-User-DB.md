
# To create a user and a database in PostgreSQL, you can follow these steps:

1. Connect to PostgreSQL: Open a command-line interface or terminal and connect to your 
  PostgreSQL database server using the psql command. You might need to provide the username 
  and password for a superuser or a user with sufficient privileges to create databases and users.

```shell
psql -U your_username -h your_host -p your_port -d your_database
```
Replace your_username, your_host, your_port, and your_database with the 
appropriate values for your PostgreSQL setup.

Create a new user: Once you're connected to the PostgreSQL server,
you can create a new user with the CREATE USER command. Specify the desired username and 
password for the user.

```sql
CREATE USER your_new_username WITH PASSWORD 'your_password';
```
Replace your_new_username with the desired username and 'your_password'
with the desired password (enclosed in single quotes).

Create a new database: After creating the user, you can create a new database with 
the CREATE DATABASE command. Assign the new user as the owner of the database using the OWNER clause.

```sql
CREATE DATABASE your_new_database OWNER your_new_username;
```
Replace your_new_database with the desired database name.

Grant privileges (optional): If you want the newly created user to have specific 
privileges on the database, such as the ability to create tables, you can grant those 
privileges using the GRANT command.

```sql
GRANT ALL PRIVILEGES ON DATABASE your_new_database TO your_new_username;
```
This example grants all privileges on the your_new_database to your_new_username.
You can adjust the privileges as needed.



## Change owner

Change the owner: Once you're connected to the PostgreSQL server, you can change the 
owner of the database using the ALTER DATABASE command. Specify the database name and the new owner.

```sql
ALTER DATABASE your_database_name OWNER TO your_new_owner;
```
