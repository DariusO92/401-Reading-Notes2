 # Day 12
 
 ## Overview
- In this quick article, we'll focus on different kinds of Spring Data repository interfaces and their functionality. We'll touch on:

 CrudRepository
- 1. PagingAndSortingRepository
 JpaRepository
- Simply put, every repository in Spring Data extends the generic Repository interface, but beyond that, they do each have different functionality.

- 2. Spring Data Repositories
Let's start with the JpaRepository – which extends PagingAndSortingRepository and, in turn, the CrudRepository.

Each of these defines its own functionality:

CrudRepository provides CRUD functions
PagingAndSortingRepository provides methods to do pagination and sort records
JpaRepository provides JPA related methods such as flushing the persistence context and delete records in a batch
And so, because of this inheritance relationship, the JpaRepository contains the full API of CrudRepository and PagingAndSortingRepository.


freestar
When we don't need the full functionality provided by JpaRepository and PagingAndSortingRepository, we can simply use the CrudRepository.

Let's now have a look at a quick example to understand these APIs better.

We'll start with a simple Product entity:

@Entity
public class Product {

    @Id
    private long id;
    private String name;

    // getters and setters
}
And let's implement a simple operation – find a Product based on its name:

@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    Product findByName(String productName);
}
That's all. The Spring Data Repository will auto-generate the implementation based on the name we provided it.


freestar
This was a very simple example of course; you can go deeper into Spring Data JPA here.

- 3. CrudRepository
Let's now have a look at the code for the CrudRepository interface:

public interface CrudRepository<T, ID extends Serializable>
  extends Repository<T, ID> {

    <S extends T> S save(S entity);

    T findOne(ID primaryKey);

    Iterable<T> findAll();

    Long count();

    void delete(T entity);

    boolean exists(ID primaryKey);
}
Notice the typical CRUD functionality:

save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
findOne(…) – get a single entity based on passed primary key value
findAll() – get an Iterable of all available entities in database
count() – return the count of total entities in a table
delete(…) – delete an entity based on the passed object
exists(…) – verify if an entity exists based on the passed primary key value
This interface looks quite generic and simple, but actually, it provides all basic query abstractions needed in an application.

- 4. PagingAndSortingRepository
Now, let's have a look at another repository interface, which extends CrudRepository:


freestar
public interface PagingAndSortingRepository<T, ID extends Serializable> 
  extends CrudRepository<T, ID> {

    Iterable<T> findAll(Sort sort);

    Page<T> findAll(Pageable pageable);
}
This interface provides a method findAll(Pageable pageable), which is the key to implementing Pagination.

When using Pageable, we create a Pageable object with certain properties and we've to specify at least:

Page size
Current page number
Sorting
So, let's assume that we want to show the first page of a result set sorted by lastName, ascending, having no more than five records each. This is how we can achieve this using a PageRequest and a Sort definition:

Sort sort = new Sort(new Sort.Order(Direction.ASC, "lastName"));
Pageable pageable = new PageRequest(0, 5, sort);
Passing the pageable object to the Spring data query will return the results in question (the first parameter of PageRequest is zero-based).

- 5. JpaRepository
Finally, we'll have a look at the JpaRepository interface:

public interface JpaRepository<T, ID extends Serializable> extends
  PagingAndSortingRepository<T, ID> {

    List<T> findAll();

    List<T> findAll(Sort sort);

    List<T> save(Iterable<? extends T> entities);

    void flush();

    T saveAndFlush(T entity);

    void deleteInBatch(Iterable<T> entities);
}
Again, let's look at each of these methods in brief:

findAll() – get a List of all available entities in database
findAll(…) – get a List of all available entities and sort them using the provided condition
save(…) – save an Iterable of entities. Here, we can pass multiple objects to save them in a batch
flush() – flush all pending task to the database
saveAndFlush(…) – save the entity and flush changes immediately
deleteInBatch(…) – delete an Iterable of entities. Here, we can pass multiple objects to delete them in a batch
Clearly, above interface extends PagingAndSortingRepository which means it has all methods present in the CrudRepository as well.

- 6. Downsides of Spring Data Repositories
Beyond all the very useful advantages of these repositories, there are some basic downsides of directly depending on these as well:


freestar
we couple our code to the library and to its specific abstractions, such as `Page` or `Pageable`; that's of course not unique to this library – but we do have to be careful not to expose these internal implementation details
by extending e.g. CrudRepository, we expose a complete set of persistence method at once. This is probably fine in most circumstances as well but we might run into situations where we'd like to gain more fine-grained control over the methods exposed, e.g. to create a ReadOnlyRepository that doesn't include the save(…) and delete(…) methods of CrudRepository

- 7. Conclusion
This article covered some brief but important differences and features of Spring Data JPA repository interfaces.
