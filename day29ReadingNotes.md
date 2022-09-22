# Day 29

## Rooms 

- Save data in a local database using Room   
Part of Android Jetpack.

bookmark_border
Apps that handle non-trivial amounts of structured data can benefit greatly from persisting that data locally. The most common use case is to cache relevant pieces of data so that when the device cannot access the network, the user can still browse that content while they are offline.

The Room persistence library provides an abstraction layer over SQLite to allow fluent database access while harnessing the full power of SQLite. In particular, Room provides the following benefits:

Compile-time verification of SQL queries.
Convenience annotations that minimize repetitive and error-prone boilerplate code.
Streamlined database migration paths.
Because of these considerations, we highly recommend that you use Room instead of using the SQLite APIs directly.

- When you use the Room persistence library to store your app's data, you define entities to represent the objects that you want to store. Each entity corresponds to a table in the associated Room database, and each instance of an entity represents a row of data in the corresponding table.

That means you can use Room entities to define your database schema without writing any SQL code.

Anatomy of an entity
You define each Room entity as a class that is annotated with @Entity. A Room entity includes fields for each column in the corresponding table in the database, including one or more columns that comprise the primary key.

- By default, Room uses the class name as the database table name. If you want the table to have a different name, set the tableName property of the @Entity annotation. Similarly, Room uses the field names as column names in the database by default.
  If you want a column to have a different name, add the @ColumnInfo annotation to the field and set the name property.
- Each Room entity must define a primary key that uniquely identifies each row in the corresponding database table. The most straightforward way of doing this is to annotate a single column with @PrimaryKey
- When you use the Room persistence library to store your app's data, you interact with the stored data by defining data access objects, or DAOs. Each DAO includes methods that offer abstract access to your app's database. At compile time, Room automatically generates implementations of the DAOs that you define.

By using DAOs to access your app's database instead of query builders or direct queries, you can preserve separation of concerns, a critical architectural principle. DAOs also make it easier for you to mock database access when you test your app.

Anatomy of a DAO
You can define each DAO as either an interface or an abstract class. For basic use cases, you should usually use an interface. In either case, you must always annotate your DAOs with @Dao. DAOs don't have properties, but they do define one or more methods for interacting with the data in your app's database.

The following code is an example of a simple DAO that defines methods for inserting, deleting, and selecting User objects in a Room database  
