# Room Database Prototype - Android
##### Tutorial: The Comprehensive Android Development Masterclass - Section 28

## Purpose: 
The purpose of this app is to explore the Room Library and how it provides a layer of abstraction over SQLite to allow database access. Specifically, I am wanting to explore the 'convenience annotations' that minimise error-prone code (SQL). The documentation highly recommendeds that you Room instead of using the SQLite APIs directly. 

##### Room Documentation: https://developer.android.com/training/data-storage/room?gclid=Cj0KCQiAlsv_BRDtARIsAHMGVSapV4bg9VhEDDfKxyhK2fuLiPLOf8n7JzNkSMuzUBRrEcU-T2The1gaAsceEALw_wcB&gclsrc=aw.ds

## Setup:
Check the documentation for the dependencies you need to add to your app's build.gradle file. You may need to revisit the version number every now and then.


## Primary Components:

### Data Entities
#### @Entity
Use this annotation in your model to let the compiler know that you want the model to be an entity.

```
@Entity(tableName = "contact_table")
public class Contact {
```

#### @PrimaryKey(autoGenerate = true)
This annotation lets the compiler know that we want the id property to also be a primary key. As we have declared the key to be auto generated, we can remove id from the constructor.

```
@PrimaryKey(autoGenerate = true)
    private int id;
    
    
    //As id is a PK, it will be automatically generate, therefore, it doesn't need to go into the constructor.
    public Contact(@NonNull String name, @NonNull String occupation) {
        this.name = name;
        this.occupation = occupation;
    }
```

#### @NonNul 
This cannotation can be used in the constructor to let the compiler know that these properties must have a value.

```
public Contact(@NonNull String name, @NonNull String occupation) {
        this.name = name;
        this.occupation = occupation;
}
```


#### This is an example of model that will also be an entity in our database:
```
@Entity(tableName = "contact_table")
public class Contact {

    @PrimaryKey(autoGenerate = true)
    private int id;

    private String name;
    private String occupation;

    public Contact() {
    }

    //As id is a PK, it will be automatically generate, therefore, it doesn't need to go into the constructor.
    public Contact(@NonNull String name, @NonNull String occupation) {
        this.name = name;
        this.occupation = occupation;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getOccupation() {
        return occupation;
    }
}
```

### Data Access Objects (DAO)
The Data Access Objects is the API for the SQLiteDatabase. It is an interface that declares the CRUD methods for your database. 

#### @Dao
You use the @Dao annotation to let the compiler know that this interface will be a Data Access Object Interface.

```
@Dao
public interface ContactDao { }
```

#### @Insert(onConflict)
The @Insert(onConflict) annotation lets the compiler know that the method is as INSERT. There are a variety of OnConflictStrategies that you can use to handle conflicts.

```
@Insert (onConflict = OnConflictStrategy.IGNORE)
void insert(Contact contact);
````

#### @Query(SQL)
The @Query annotations lets the know that the method is a query, and more specifically, what kind of query the method is performing. You use SQL to declare your intentions for each of the query methods. 
```
@Query("DELETE FROM contact_table")
void deleteAll();

@Query("SELECT * FROM contact_table ORDER BY name ASC")
LiveData<List<Contact>> getAllContacts();
```

#### This is an example of a DAO interface:
```
@Dao
public interface ContactDao {

    //CRUD Operations
    @Insert (onConflict = OnConflictStrategy.IGNORE)
    void insert(Contact contact);

    @Query("DELETE FROM contact_table")
    void deleteAll();

    @Query("SELECT * FROM contact_table ORDER BY name ASC")
    LiveData<List<Contact>> getAllContacts();

}
```



### LiveData
