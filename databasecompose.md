
### Adding a Database for Compose

Learn how to add a database to your Compose based environment.If you’re not using Compose, follow the databases for Dockerfiles guide.Start by creating a Dockerfile to build your service and adding it to your repository:MySQL   PostgreSQL   Dockerfile-db

```
# Change version number to desired (i.e. 5.5, 5.6, 5.7)
FROM runnable/mysql:5.6

# Set desired environment variables
ENV MYSQL_USER mysql
ENV MYSQL_PASSWORD mysql
ENV MYSQL_DATABASE app
# to set a root password, uncomment the next line
# ENV MYSQL_ROOT_PASSWORD test

# Uncomment the following ADD line to enable seeding the PostgreSQL DB
# Make sure to check in a mysql dump file (i.e. mysqldump [options] > seed.sql)
# ADD seed.sql /seed.sql

# Run the initialization script (leave this alone)
RUN gosu mysql /init.sh
```

Then add your database service to your Compose file:

Dockerfile-db

```
Database: ./Dockerfile-db
```

Once you push your changes to your repository, you should see a new database service launch on the Containers page.Note: If you’re using Docker Hub’s MySQL image, your output may differ due to their use of entrypoint and volumes, which are unsupported features.

#### Seeding Your Database

Creating a Seed FileYou can clone an existing database to create a seed file. Here’s how to create one:

MySQL

`$ mysqldump --all-databases -u mysql -p > seed.sql`

Note: The default password is mysql.You are now the proud owner of a `seed.sqlseed.dump` file.

#### Using the Seed File

Your database’s Dockerfile will need access to your seed file. Check the file into your repository and add this line to the Dockerfile before the `RUN` command:

MySQL

Dockerfile-db

`ADD [src] /seed.sql`

Replace `[src]` with the path to your dump file, relative to your Dockerfile:

Note: Do not modify the destination `/seed.sql`; it’s required to for the initialization script.Your seeded database will now be created for all new builds.
