# Java: Androidx Room Common

- [ColumnInfo](#columninfo)
- [Dao](#dao)
- [Database](#database)
- [Delete](#delete)
- [Embedded](#embedded)
- [Entity](#entity)
- [ForeignKey](#foreignkey)
- [Ignore](#ignore)
- [Index](#index)
- [Insert](#insert)
- [OnConflictStrategy](#onconflictstrategy)
- [PrimaryKey](#primarykey)
- [Query](#query)
- [RawQuery](#rawquery)
- [Relation](#relation)
- [RoomMasterTable](#roommastertable)
- [RoomWarnings](#roomwarnings)
- [SkipQueryVerification](#skipqueryverification)
- [Transaction](#transaction)
- [TypeConverter](#typeconverter)
- [TypeConverters](#typeconverters)
- [Update](#update)

## ColumnInfo


```java

package androidx.room;
import androidx.annotation.IntDef;
import androidx.annotation.RequiresApi;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Allows specific customization about the column associated with this field.
 * <p>
 * For example, you can specify a column name for the field or change the column's type affinity.
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.CLASS)
public @interface ColumnInfo {
    /**
     * Name of the column in the database. Defaults to the field name if not set.
     *
     * @return Name of the column in the database.
     */
    String name() default INHERIT_FIELD_NAME;
    /**
     * The type affinity for the column, which will be used when constructing the database.
     * <p>
     * If it is not specified, the value defaults to {@link #UNDEFINED} and Room resolves it based
     * on the field's type and available TypeConverters.
     * <p>
     * See <a href="https://www.sqlite.org/datatype3.html">SQLite types documentation</a> for
     * details.
     *
     * @return The type affinity of the column. This is either {@link #UNDEFINED}, {@link #TEXT},
     * {@link #INTEGER}, {@link #REAL}, or {@link #BLOB}.
     */
    @SuppressWarnings("unused") @SQLiteTypeAffinity int typeAffinity() default UNDEFINED;
    /**
     * Convenience method to index the field.
     * <p>
     * If you would like to create a composite index instead, see: {@link Index}.
     *
     * @return True if this field should be indexed, false otherwise. Defaults to false.
     */
    boolean index() default false;
    /**
     * The collation sequence for the column, which will be used when constructing the database.
     * <p>
     * The default value is {@link #UNSPECIFIED}. In that case, Room does not add any
     * collation sequence to the column, and SQLite treats it like {@link #BINARY}.
     *
     * @return The collation sequence of the column. This is either {@link #UNSPECIFIED},
     * {@link #BINARY}, {@link #NOCASE}, {@link #RTRIM}, {@link #LOCALIZED} or {@link #UNICODE}.
     */
    @Collate int collate() default UNSPECIFIED;
    /**
     * Constant to let Room inherit the field name as the column name. If used, Room will use the
     * field name as the column name.
     */
    String INHERIT_FIELD_NAME = "[field-name]";
    /**
     * Undefined type affinity. Will be resolved based on the type.
     *
     * @see #typeAffinity()
     */
    int UNDEFINED = 1;
    /**
     * Column affinity constant for strings.
     *
     * @see #typeAffinity()
     */
    int TEXT = 2;
    /**
     * Column affinity constant for integers or booleans.
     *
     * @see #typeAffinity()
     */
    int INTEGER = 3;
    /**
     * Column affinity constant for floats or doubles.
     *
     * @see #typeAffinity()
     */
    int REAL = 4;
    /**
     * Column affinity constant for binary data.
     *
     * @see #typeAffinity()
     */
    int BLOB = 5;
    /**
     * The SQLite column type constants that can be used in {@link #typeAffinity()}
     */
    @IntDef({UNDEFINED, TEXT, INTEGER, REAL, BLOB})
    @interface SQLiteTypeAffinity {
    }
    /**
     * Collation sequence is not specified. The match will behave like {@link #BINARY}.
     *
     * @see #collate()
     */
    int UNSPECIFIED = 1;
    /**
     * Collation sequence for case-sensitive match.
     *
     * @see #collate()
     */
    int BINARY = 2;
    /**
     * Collation sequence for case-insensitive match.
     *
     * @see #collate()
     */
    int NOCASE = 3;
    /**
     * Collation sequence for case-sensitive match except that trailing space characters are
     * ignored.
     *
     * @see #collate()
     */
    int RTRIM = 4;
    /**
     * Collation sequence that uses system's current locale.
     *
     * @see #collate()
     */
    @RequiresApi(21)
    int LOCALIZED = 5;
    /**
     * Collation sequence that uses Unicode Collation Algorithm.
     *
     * @see #collate()
     */
    @RequiresApi(21)
    int UNICODE = 6;
    @IntDef({UNSPECIFIED, BINARY, NOCASE, RTRIM, LOCALIZED, UNICODE})
    @interface Collate {
    }
}
```

## Dao


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks the class as a Data Access Object.
 * <p>
 * Data Access Objects are the main classes where you define your database interactions. They can
 * include a variety of query methods.
 * <p>
 * The class marked with {@code @Dao} should either be an interface or an abstract class. At compile
 * time, Room will generate an implementation of this class when it is referenced by a
 * {@link Database}.
 * <p>
 * An abstract {@code @Dao} class can optionally have a constructor that takes a {@link Database}
 * as its only parameter.
 * <p>
 * It is recommended to have multiple {@code Dao} classes in your codebase depending on the tables
 * they touch.
 *
 * @see Query
 * @see Delete
 * @see Insert
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.CLASS)
public @interface Dao {
}
```

## Database


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a class as a RoomDatabase.
 * <p>
 * The class should be an abstract class and extend
 * {@link androidx.room.RoomDatabase RoomDatabase}.
 * <p>
 * You can receive an implementation of the class via
 * {@link androidx.room.Room#databaseBuilder Room.databaseBuilder} or
 * {@link androidx.room.Room#inMemoryDatabaseBuilder Room.inMemoryDatabaseBuilder}.
 * <p>
 * <pre>
 * // User and Book are classes annotated with {@literal @}Entity.
 * {@literal @}Database(version = 1, entities = {User.class, Book.class})
 * abstract class AppDatabase extends RoomDatabase {
 *     // BookDao is a class annotated with {@literal @}Dao.
 *     abstract public BookDao bookDao();
 *     // UserDao is a class annotated with {@literal @}Dao.
 *     abstract public UserDao userDao();
 *     // UserBookDao is a class annotated with {@literal @}Dao.
 *     abstract public UserBookDao userBookDao();
 * }
 * </pre>
 * The example above defines a class that has 2 tables and 3 DAO classes that are used to access it.
 * There is no limit on the number of {@link Entity} or {@link Dao} classes but they must be unique
 * within the Database.
 * <p>
 * Instead of running queries on the database directly, you are highly recommended to create
 * {@link Dao} classes. Using Dao classes will allow you to abstract the database communication in
 * a more logical layer which will be much easier to mock in tests (compared to running direct
 * sql queries). It also automatically does the conversion from {@code Cursor} to your application
 * classes so you don't need to deal with lower level database APIs for most of your data access.
 * <p>
 * Room also verifies all of your queries in {@link Dao} classes while the application is being
 * compiled so that if there is a problem in one of the queries, you will be notified instantly.
 * @see Dao
 * @see Entity
 * @see androidx.room.RoomDatabase RoomDatabase
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.CLASS)
public @interface Database {
    /**
     * The list of entities included in the database. Each entity turns into a table in the
     * database.
     *
     * @return The list of entities in the database.
     */
    Class[] entities();
    /**
     * The database version.
     *
     * @return The database version.
     */
    int version();
    /**
     * You can set annotation processor argument ({@code room.schemaLocation})
     * to tell Room to export the schema into a folder. Even though it is not mandatory, it is a
     * good practice to have version history in your codebase and you should commit that file into
     * your version control system (but don't ship it with your app!).
     * <p>
     * When {@code room.schemaLocation} is set, Room will check this variable and if it is set to
     * {@code true}, its schema will be exported into the given folder.
     * <p>
     * {@code exportSchema} is {@code true} by default but you can disable it for databases when
     * you don't want to keep history of versions (like an in-memory only database).
     *
     * @return Whether the schema should be exported to the given folder when the
     * {@code room.schemaLocation} argument is set. Defaults to {@code true}.
     */
    boolean exportSchema() default true;
}
```

## Delete


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method in a {@link Dao} annotated class as a delete method.
 * <p>
 * The implementation of the method will delete its parameters from the database.
 * <p>
 * All of the parameters of the Delete method must either be classes annotated with {@link Entity}
 * or collections/array of it.
 * <p>
 * Example:
 * <pre>
 * {@literal @}Dao
 * public interface MyDao {
 *     {@literal @}Delete
 *     public void deleteUsers(User... users);
 *     {@literal @}Delete
 *     public void deleteAll(User user1, User user2);
 *     {@literal @}Delete
 *     public void deleteWithFriends(User user, List&lt;User&gt; friends);
 * }
 * </pre>
 *
 * @see Insert
 * @see Query
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.CLASS)
public @interface Delete {
}
```

## Embedded


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Can be used as an annotation on a field of an {@link Entity} or {@code Pojo} to signal that
 * nested fields (i.e. fields of the annotated field's class) can be referenced directly in the SQL
 * queries.
 * <p>
 * If the container is an {@link Entity}, these sub fields will be columns in the {@link Entity}'s
 * database table.
 * <p>
 * For example, if you have 2 classes:
 * <pre>
 *   public class Coordinates {
 *       double latitude;
 *       double longitude;
 *   }
 *   public class Address {
 *       String street;
 *       {@literal @}Embedded
 *       Coordinates coordinates;
 *   }
 * </pre>
 * Room will consider {@code latitude} and {@code longitude} as if they are fields of the
 * {@code Address} class when mapping an SQLite row to {@code Address}.
 * <p>
 * So if you have a query that returns {@code street, latitude, longitude}, Room will properly
 * construct an {@code Address} class.
 * <p>
 * If the {@code Address} class is annotated with {@link Entity}, its database table will have 3
 * columns: {@code street, latitude, longitude}
 * <p>
 * If there is a name conflict with the fields of the sub object and the owner object, you can
 * specify a {@link #prefix()} for the items of the sub object. Note that prefix is always applied
 * to sub fields even if they have a {@link ColumnInfo} with a specific {@code name}.
 * <p>
 * If sub fields of an embedded field has {@link PrimaryKey} annotation, they <b>will not</b> be
 * considered as primary keys in the owner {@link Entity}.
 * <p>
 * When an embedded field is read, if all fields of the embedded field (and its sub fields) are
 * {@code null} in the {@link android.database.Cursor Cursor}, it is set to {@code null}. Otherwise,
 * it is constructed.
 * <p>
 * Note that even if you have {@link TypeConverter}s that convert a {@code null} column into a
 * {@code non-null} value, if all columns of the embedded field in the
 * {@link android.database.Cursor Cursor} are null, the {@link TypeConverter} will never be called
 * and the embedded field will not be constructed.
 * <p>
 * You can override this behavior by annotating the embedded field with
 * {@link androidx.annotation.NonNull}.
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.CLASS)
public @interface Embedded {
    /**
     * Specifies a prefix to prepend the column names of the fields in the embedded fields.
     * <p>
     * For the example above, if we've written:
     * <pre>
     *   {@literal @}Embedded(prefix = "foo_")
     *   Coordinates coordinates;
     * </pre>
     * The column names for {@code latitude} and {@code longitude} will be {@code foo_latitude} and
     * {@code foo_longitude} respectively.
     * <p>
     * By default, prefix is the empty string.
     *
     * @return The prefix to be used for the fields of the embedded item.
     */
    String prefix() default  "";
}
```

## Entity


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a class as an entity. This class will have a mapping SQLite table in the database.
 * <p>
 * Each entity must have at least 1 field annotated with {@link PrimaryKey}.
 * You can also use {@link #primaryKeys()} attribute to define the primary key.
 * <p>
 * Each entity must either have a no-arg constructor or a constructor whose parameters match
 * fields (based on type and name). Constructor does not have to receive all fields as parameters
 * but if a field is not passed into the constructor, it should either be public or have a public
 * setter. If a matching constructor is available, Room will always use it. If you don't want it
 * to use a constructor, you can annotate it with {@link Ignore}.
 * <p>
 * When a class is marked as an Entity, all of its fields are persisted. If you would like to
 * exclude some of its fields, you can mark them with {@link Ignore}.
 * <p>
 * If a field is {@code transient}, it is automatically ignored <b>unless</b> it is annotated with
 * {@link ColumnInfo}, {@link Embedded} or {@link Relation}.
 * <p>
 * Example:
 * <pre>
 * {@literal @}Entity
 * public class User {
 *   {@literal @}PrimaryKey
 *   private final int uid;
 *   private String name;
 *   {@literal @}ColumnInfo(name = "last_name")
 *   private String lastName;
 *
 *   public User(int uid) {
 *       this.uid = uid;
 *   }
 *   public String getLastName() {
 *       return lastName;
 *   }
 *   public void setLastName(String lastName) {
 *       this.lastName = lastName;
 *   }
 * }
 * </pre>
 *
 * @see Dao
 * @see Database
 * @see PrimaryKey
 * @see ColumnInfo
 * @see Index
 */
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.CLASS)
public @interface Entity {
    /**
     * The table name in the SQLite database. If not set, defaults to the class name.
     *
     * @return The SQLite tableName of the Entity.
     */
    String tableName() default "";
    /**
     * List of indices on the table.
     *
     * @return The list of indices on the table.
     */
    Index[] indices() default {};
    /**
     * If set to {@code true}, any Index defined in parent classes of this class will be carried
     * over to the current {@code Entity}. Note that if you set this to {@code true}, even if the
     * {@code Entity} has a parent which sets this value to {@code false}, the {@code Entity} will
     * still inherit indices from it and its parents.
     * <p>
     * When the {@code Entity} inherits an index from the parent, it is <b>always</b> renamed with
     * the default naming schema since SQLite <b>does not</b> allow using the same index name in
     * multiple tables. See {@link Index} for the details of the default name.
     * <p>
     * By default, indices defined in parent classes are dropped to avoid unexpected indices.
     * When this happens, you will receive a {@link RoomWarnings#INDEX_FROM_PARENT_FIELD_IS_DROPPED}
     * or {@link RoomWarnings#INDEX_FROM_PARENT_IS_DROPPED} warning during compilation.
     *
     * @return True if indices from parent classes should be automatically inherited by this Entity,
     *         false otherwise. Defaults to false.
     */
    boolean inheritSuperIndices() default false;
    /**
     * The list of Primary Key column names.
     * <p>
     * If you would like to define an auto generated primary key, you can use {@link PrimaryKey}
     * annotation on the field with {@link PrimaryKey#autoGenerate()} set to {@code true}.
     *
     * @return The primary key of this Entity. Can be empty if the class has a field annotated
     * with {@link PrimaryKey}.
     */
    String[] primaryKeys() default {};
    /**
     * List of {@link ForeignKey} constraints on this entity.
     *
     * @return The list of {@link ForeignKey} constraints on this entity.
     */
    ForeignKey[] foreignKeys() default {};
}
```

## ForeignKey


```java

package androidx.room;
import static java.lang.annotation.RetentionPolicy.SOURCE;
import androidx.annotation.IntDef;
import java.lang.annotation.Retention;
/**
 * Declares a foreign key on another {@link Entity}.
 * <p>
 * Foreign keys allows you to specify constraints across Entities such that SQLite will ensure that
 * the relationship is valid when you modify the database.
 * <p>
 * When a foreign key constraint is specified, SQLite requires the referenced columns to be part of
 * a unique index in the parent table or the primary key of that table. You must create a unique
 * index in the parent entity that covers the referenced columns (Room will verify this at compile
 * time and print an error if it is missing).
 * <p>
 * It is also recommended to create an index on the child table to avoid full table scans when the
 * parent table is modified. If a suitable index on the child table is missing, Room will print
 * {@link RoomWarnings#MISSING_INDEX_ON_FOREIGN_KEY_CHILD} warning.
 * <p>
 * A foreign key constraint can be deferred until the transaction is complete. This is useful if
 * you are doing bulk inserts into the database in a single transaction. By default, foreign key
 * constraints are immediate but you can change this value by setting {@link #deferred()} to
 * {@code true}. You can also use
 * <a href="https://sqlite.org/pragma.html#pragma_defer_foreign_keys">defer_foreign_keys</a> PRAGMA
 * to defer them depending on your transaction.
 * <p>
 * Please refer to the SQLite <a href="https://sqlite.org/foreignkeys.html">foreign keys</a>
 * documentation for details.
 */
public @interface ForeignKey {
    /**
     * The parent Entity to reference. It must be a class annotated with {@link Entity} and
     * referenced in the same database.
     *
     * @return The parent Entity.
     */
    Class entity();
    /**
     * The list of column names in the parent {@link Entity}.
     * <p>
     * Number of columns must match the number of columns specified in {@link #childColumns()}.
     *
     * @return The list of column names in the parent Entity.
     * @see #childColumns()
     */
    String[] parentColumns();
    /**
     * The list of column names in the current {@link Entity}.
     * <p>
     * Number of columns must match the number of columns specified in {@link #parentColumns()}.
     *
     * @return The list of column names in the current Entity.
     */
    String[] childColumns();
    /**
     * Action to take when the parent {@link Entity} is deleted from the database.
     * <p>
     * By default, {@link #NO_ACTION} is used.
     *
     * @return The action to take when the referenced entity is deleted from the database.
     */
    @Action int onDelete() default NO_ACTION;
    /**
     * Action to take when the parent {@link Entity} is updated in the database.
     * <p>
     * By default, {@link #NO_ACTION} is used.
     *
     * @return The action to take when the referenced entity is updated in the database.
     */
    @Action int onUpdate() default NO_ACTION;
    /**
     * * A foreign key constraint can be deferred until the transaction is complete. This is useful
     * if you are doing bulk inserts into the database in a single transaction. By default, foreign
     * key constraints are immediate but you can change it by setting this field to {@code true}.
     * You can also use
     * <a href="https://sqlite.org/pragma.html#pragma_defer_foreign_keys">defer_foreign_keys</a>
     * PRAGMA to defer them depending on your transaction.
     *
     * @return Whether the foreign key constraint should be deferred until the transaction is
     * complete. Defaults to {@code false}.
     */
    boolean deferred() default false;
    /**
     * Possible value for {@link #onDelete()} or {@link #onUpdate()}.
     * <p>
     * When a parent key is modified or deleted from the database, no special action is taken.
     * This means that SQLite will not make any effort to fix the constraint failure, instead,
     * reject the change.
     */
    int NO_ACTION = 1;
    /**
     * Possible value for {@link #onDelete()} or {@link #onUpdate()}.
     * <p>
     * The RESTRICT action means that the application is prohibited from deleting
     * (for {@link #onDelete()}) or modifying (for {@link #onUpdate()}) a parent key when there
     * exists one or more child keys mapped to it. The difference between the effect of a RESTRICT
     * action and normal foreign key constraint enforcement is that the RESTRICT action processing
     * happens as soon as the field is updated - not at the end of the current statement as it would
     * with an immediate constraint, or at the end of the current transaction as it would with a
     * {@link #deferred()} constraint.
     * <p>
     * Even if the foreign key constraint it is attached to is {@link #deferred()}, configuring a
     * RESTRICT action causes SQLite to return an error immediately if a parent key with dependent
     * child keys is deleted or modified.
     */
    int RESTRICT = 2;
    /**
     * Possible value for {@link #onDelete()} or {@link #onUpdate()}.
     * <p>
     * If the configured action is "SET NULL", then when a parent key is deleted
     * (for {@link #onDelete()}) or modified (for {@link #onUpdate()}), the child key columns of all
     * rows in the child table that mapped to the parent key are set to contain {@code NULL} values.
     */
    int SET_NULL = 3;
    /**
     * Possible value for {@link #onDelete()} or {@link #onUpdate()}.
     * <p>
     * The "SET DEFAULT" actions are similar to {@link #SET_NULL}, except that each of the child key
     * columns is set to contain the columns default value instead of {@code NULL}.
     */
    int SET_DEFAULT = 4;
    /**
     * Possible value for {@link #onDelete()} or {@link #onUpdate()}.
     * <p>
     * A "CASCADE" action propagates the delete or update operation on the parent key to each
     * dependent child key. For {@link #onDelete()} action, this means that each row in the child
     * entity that was associated with the deleted parent row is also deleted. For an
     * {@link #onUpdate()} action, it means that the values stored in each dependent child key are
     * modified to match the new parent key values.
     */
    int CASCADE = 5;
    /**
     * Constants definition for values that can be used in {@link #onDelete()} and
     * {@link #onUpdate()}.
     */
    @IntDef({NO_ACTION, RESTRICT, SET_NULL, SET_DEFAULT, CASCADE})
    @Retention(SOURCE)
    @interface Action {
    }
}
```

## Ignore


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Ignores the marked element from Room's processing logic.
 * <p>
 * This annotation can be used in multiple places where Room processor runs. For instance, you can
 * add it to a field of an {@link Entity} and Room will not persist that field.
 */
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.CONSTRUCTOR})
@Retention(RetentionPolicy.CLASS)
public @interface Ignore {
}
```

## Index


```java

package androidx.room;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Declares an index on an Entity.
 * see: <a href="https://sqlite.org/lang_createindex.html">SQLite Index Documentation</a>
 * <p>
 * Adding an index usually speeds up your select queries but will slow down other queries like
 * insert or update. You should be careful when adding indices to ensure that this additional cost
 * is worth the gain.
 * <p>
 * There are 2 ways to define an index in an {@link Entity}. You can either set
 * {@link ColumnInfo#index()} property to index individual fields or define composite indices via
 * {@link Entity#indices()}.
 * <p>
 * If an indexed field is embedded into another Entity via {@link Embedded}, it is <b>NOT</b>
 * added as an index to the containing {@link Entity}. If you want to keep it indexed, you must
 * re-declare it in the containing {@link Entity}.
 * <p>
 * Similarly, if an {@link Entity} extends another class, indices from the super classes are
 * <b>NOT</b> inherited. You must re-declare them in the child {@link Entity} or set
 * {@link Entity#inheritSuperIndices()} to {@code true}.
 * */
@Target({})
@Retention(RetentionPolicy.CLASS)
public @interface Index {
    /**
     * List of column names in the Index.
     * <p>
     * The order of columns is important as it defines when SQLite can use a particular index.
     * See <a href="https://www.sqlite.org/optoverview.html">SQLite documentation</a> for details on
     * index usage in the query optimizer.
     *
     * @return The list of column names in the Index.
     */
    String[] value();
    /**
     * Name of the index. If not set, Room will set it to the list of columns joined by '_' and
     * prefixed by "index_${tableName}". So if you have a table with name "Foo" and with an index
     * of {"bar", "baz"}, generated index name will be  "index_Foo_bar_baz". If you need to specify
     * the index in a query, you should never rely on this name, instead, specify a name for your
     * index.
     *
     * @return The name of the index.
     */
    String name() default "";
    /**
     * If set to true, this will be a unique index and any duplicates will be rejected.
     *
     * @return True if index is unique. False by default.
     */
    boolean unique() default false;
}
```

## Insert


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method in a {@link Dao} annotated class as an insert method.
 * <p>
 * The implementation of the method will insert its parameters into the database.
 * <p>
 * All of the parameters of the Insert method must either be classes annotated with {@link Entity}
 * or collections/array of it.
 * <p>
 * Example:
 * <pre>
 * {@literal @}Dao
 * public interface MyDao {
 *     {@literal @}Insert(onConflict = OnConflictStrategy.REPLACE)
 *     public void insertUsers(User... users);
 *     {@literal @}Insert
 *     public void insertBoth(User user1, User user2);
 *     {@literal @}Insert
 *     public void insertWithFriends(User user, List&lt;User&gt; friends);
 * }
 * </pre>
 *
 * @see Update
 * @see Delete
 */
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.CLASS)
public @interface Insert {
    /**
     * What to do if a conflict happens.
     * @see <a href="https://sqlite.org/lang_conflict.html">SQLite conflict documentation</a>
     *
     * @return How to handle conflicts. Defaults to {@link OnConflictStrategy#ABORT}.
     */
    @OnConflictStrategy
    int onConflict() default OnConflictStrategy.ABORT;
}
```

## OnConflictStrategy


```java

package androidx.room;
import static java.lang.annotation.RetentionPolicy.SOURCE;
import androidx.annotation.IntDef;
import java.lang.annotation.Retention;
/**
 * Set of conflict handling strategies for various {@link Dao} methods.
 * <p>
 * Check <a href="https://sqlite.org/lang_conflict.html">SQLite conflict documentation</a> for
 * details.
 */
@Retention(SOURCE)
@IntDef({OnConflictStrategy.REPLACE, OnConflictStrategy.ROLLBACK, OnConflictStrategy.ABORT,
        OnConflictStrategy.FAIL, OnConflictStrategy.IGNORE})
public @interface OnConflictStrategy {
    /**
     * OnConflict strategy constant to replace the old data and continue the transaction.
     */
    int REPLACE = 1;
    /**
     * OnConflict strategy constant to rollback the transaction.
     */
    int ROLLBACK = 2;
    /**
     * OnConflict strategy constant to abort the transaction.
     */
    int ABORT = 3;
    /**
     * OnConflict strategy constant to fail the transaction.
     */
    int FAIL = 4;
    /**
     * OnConflict strategy constant to ignore the conflict.
     */
    int IGNORE = 5;
}
```

## PrimaryKey


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a field in an {@link Entity} as the primary key.
 * <p>
 * If you would like to define a composite primary key, you should use {@link Entity#primaryKeys()}
 * method.
 * <p>
 * Each {@link Entity} must declare a primary key unless one of its super classes declares a
 * primary key. If both an {@link Entity} and its super class defines a {@code PrimaryKey}, the
 * child's {@code PrimaryKey} definition will override the parent's {@code PrimaryKey}.
 * <p>
 * If {@code PrimaryKey} annotation is used on a {@link Embedded}d field, all columns inherited
 * from that embedded field becomes the composite primary key (including its grand children
 * fields).
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.CLASS)
public @interface PrimaryKey {
    /**
     * Set to true to let SQLite generate the unique id.
     * <p>
     * When set to {@code true}, the SQLite type affinity for the field should be {@code INTEGER}.
     * <p>
     * If the field type is {@code long} or {@code int} (or its TypeConverter converts it to a
     * {@code long} or {@code int}), {@link Insert} methods treat {@code 0} as not-set while
     * inserting the item.
     * <p>
     * If the field's type is {@link Integer} or {@link Long} (or its TypeConverter converts it to
     * an {@link Integer} or a {@link Long}), {@link Insert} methods treat {@code null} as
     * not-set while inserting the item.
     *
     * @return Whether the primary key should be auto-generated by SQLite or not. Defaults
     * to false.
     */
    boolean autoGenerate() default false;
}
```

## Query


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method in a {@link Dao} annotated class as a query method.
 * <p>
 * The value of the annotation includes the query that will be run when this method is called. This
 * query is <b>verified at compile time</b> by Room to ensure that it compiles fine against the
 * database.
 * <p>
 * The arguments of the method will be bound to the bind arguments in the SQL statement. See
 * <href="https://www.sqlite.org/c3ref/bind_blob.html">SQLite's binding documentation</> for
 * details of bind arguments in SQLite.
 * <p>
 * Room only supports named bind parameter {@code :name} to avoid any confusion between the
 * method parameters and the query bind parameters.
 * <p>
 * Room will automatically bind the parameters of the method into the bind arguments. This is done
 * by matching the name of the parameters to the name of the bind arguments.
 * <pre>
 *     {@literal @}Query("SELECT * FROM user WHERE user_name LIKE :name AND last_name LIKE :last")
 *     public abstract List&lt;User&gt; findUsersByNameAndLastName(String name, String last);
 * </pre>
 * <p>
 * As an extension over SQLite bind arguments, Room supports binding a list of parameters to the
 * query. At runtime, Room will build the correct query to have matching number of bind arguments
 * depending on the number of items in the method parameter.
 * <pre>
 *     {@literal @}Query("SELECT * FROM user WHERE uid IN(:userIds)")
 *     public abstract List<User> findByIds(int[] userIds);
 * </pre>
 * For the example above, if the {@code userIds} is an array of 3 elements, Room will run the
 * query as: {@code SELECT * FROM user WHERE uid IN(?, ?, ?)} and bind each item in the
 * {@code userIds} array into the statement.
 * <p>
 * There are 3 types of queries supported in {@code Query} methods: SELECT, UPDATE and DELETE.
 * <p>
 * For SELECT queries, Room will infer the result contents from the method's return type and
 * generate the code that will automatically convert the query result into the method's return
 * type. For single result queries, the return type can be any java object. For queries that return
 * multiple values, you can use {@link java.util.List} or {@code Array}. In addition to these, any
 * query may return {@link android.database.Cursor Cursor} or any query result can be wrapped in
 * a {@link androidx.lifecycle.LiveData LiveData}.
 * <p>
 * <b>RxJava2</b> If you are using RxJava2, you can also return {@code Flowable<T>} or
 * {@code Publisher<T>} from query methods. Since Reactive Streams does not allow {@code null}, if
 * the query returns a nullable type, it will not dispatch anything if the value is {@code null}
 * (like fetching an {@link Entity} row that does not exist).
 * You can return {@code Flowable<T[]>} or {@code Flowable<List<T>>} to workaround this limitation.
 * <p>
 * Both {@code Flowable<T>} and {@code Publisher<T>} will observe the database for changes and
 * re-dispatch if data changes. If you want to query the database without observing changes, you can
 * use {@code Maybe<T>} or {@code Single<T>}. If a {@code Single<T>} query returns {@code null},
 * Room will throw
 * {@link androidx.room.EmptyResultSetException EmptyResultSetException}.
 * <p>
 * UPDATE or DELETE queries can return {@code void} or {@code int}. If it is an {@code int},
 * the value is the number of rows affected by this query.
 * <p>
 * You can return arbitrary POJOs from your query methods as long as the fields of the POJO match
 * the column names in the query result.
 * For example, if you have class:
 * <pre>
 * class UserName {
 *     public String name;
 *     {@literal @}ColumnInfo(name = "last_name")
 *     public String lastName;
 * }
 * </pre>
 * You can write a query like this:
 * <pre>
 *     {@literal @}Query("SELECT last_name, name FROM user WHERE uid = :userId LIMIT 1")
 *     public abstract UserName findOneUserName(int userId);
 * </pre>
 * And Room will create the correct implementation to convert the query result into a
 * {@code UserName} object. If there is a mismatch between the query result and the fields of the
 * POJO, as long as there is at least 1 field match, Room prints a
 * {@link RoomWarnings#CURSOR_MISMATCH} warning and sets as many fields as it can.
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.CLASS)
public @interface Query {
    /**
     * The SQLite query to be run.
     * @return The query to be run.
     */
    String value();
}
```

## RawQuery


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method in a {@link Dao} annotated class as a raw query method where you can pass the
 * query as a {@link androidx.sqlite.db.SupportSQLiteQuery SupportSQLiteQuery}.
 * <pre>
 * {@literal @}Dao
 * interface RawDao {
 *     {@literal @}RawQuery
 *     User getUserViaQuery(SupportSQLiteQuery query);
 * }
 * SimpleSQLiteQuery query = new SimpleSQLiteQuery("SELECT * FROM User WHERE id = ? LIMIT 1",
 *         new Object[]{userId});
 * User user2 = rawDao.getUserViaQuery(query);
 * </pre>
 * <p>
 * Room will generate the code based on the return type of the function and failure to
 * pass a proper query will result in a runtime failure or an undefined result.
 * <p>
 * If you know the query at compile time, you should always prefer {@link Query} since it validates
 * the query at compile time and also generates more efficient code since Room can compute the
 * query result at compile time (e.g. it does not need to account for possibly missing columns in
 * the response).
 * <p>
 * On the other hand, {@code RawQuery} serves as an escape hatch where you can build your own
 * SQL query at runtime but still use Room to convert it into objects.
 * <p>
 * {@code RawQuery} methods must return a non-void type. If you want to execute a raw query that
 * does not return any value, use {@link androidx.room.RoomDatabase#query
 * RoomDatabase#query} methods.
 * <p>
 * RawQuery methods can only be used for read queries. For write queries, use
 * {@link androidx.room.RoomDatabase#getOpenHelper
 * RoomDatabase.getOpenHelper().getWritableDatabase()}.
 * <p>
 * <b>Observable Queries:</b>
 * <p>
 * {@code RawQuery} methods can return observable types but you need to specify which tables are
 * accessed in the query using the {@link #observedEntities()} field in the annotation.
 * <pre>
 * {@literal @}Dao
 * interface RawDao {
 *     {@literal @}RawQuery(observedEntities = User.class)
 *     LiveData&lt;List&lt;User>> getUsers(SupportSQLiteQuery query);
 * }
 * LiveData&lt;List&lt;User>> liveUsers = rawDao.getUsers(
 *     new SimpleSQLiteQuery("SELECT * FROM User ORDER BY name DESC"));
 * </pre>
 * <b>Returning Pojos:</b>
 * <p>
 * RawQueries can also return plain old java objects, similar to {@link Query} methods.
 * <pre>
 * public class NameAndLastName {
 *     public final String name;
 *     public final String lastName;
 *
 *     public NameAndLastName(String name, String lastName) {
 *         this.name = name;
 *         this.lastName = lastName;
 *     }
 * }
 *
 * {@literal @}Dao
 * interface RawDao {
 *     {@literal @}RawQuery
 *     NameAndLastName getNameAndLastName(SupportSQLiteQuery query);
 * }
 * NameAndLastName result = rawDao.getNameAndLastName(
 *      new SimpleSQLiteQuery("SELECT * FROM User WHERE id = ?", new Object[]{userId}))
 * // or
 * NameAndLastName result = rawDao.getNameAndLastName(
 *      new SimpleSQLiteQuery("SELECT name, lastName FROM User WHERE id = ?",
 *          new Object[]{userId})))
 * </pre>
 * <p>
 * <b>Pojos with Embedded Fields:</b>
 * <p>
 * {@code RawQuery} methods can return pojos that include {@link Embedded} fields as well.
 * <pre>
 * public class UserAndPet {
 *     {@literal @}Embedded
 *     public User user;
 *     {@literal @}Embedded
 *     public Pet pet;
 * }
 *
 * {@literal @}Dao
 * interface RawDao {
 *     {@literal @}RawQuery
 *     UserAndPet getUserAndPet(SupportSQLiteQuery query);
 * }
 * UserAndPet received = rawDao.getUserAndPet(
 *         new SimpleSQLiteQuery("SELECT * FROM User, Pet WHERE User.id = Pet.userId LIMIT 1"))
 * </pre>
 *
 * <b>Relations:</b>
 * <p>
 * {@code RawQuery} return types can also be objects with {@link Relation Relations}.
 * <pre>
 * public class UserAndAllPets {
 *     {@literal @}Embedded
 *     public User user;
 *     {@literal @}Relation(parentColumn = "id", entityColumn = "userId")
 *     public List&lt;Pet> pets;
 * }
 *
 * {@literal @}Dao
 * interface RawDao {
 *     {@literal @}RawQuery
 *     List&lt;UserAndAllPets> getUsersAndAllPets(SupportSQLiteQuery query);
 * }
 * List&lt;UserAndAllPets> result = rawDao.getUsersAndAllPets(
 *      new SimpleSQLiteQuery("SELECT * FROM users"));
 * </pre>
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.CLASS)
public @interface RawQuery {
    /**
     * Denotes the list of entities which are accessed in the provided query and should be observed
     * for invalidation if the query is observable.
     * <p>
     * The listed classes should either be annotated with {@link Entity} or they should reference to
     * at least 1 Entity (via {@link Embedded} or {@link Relation}).
     * <p>
     * Providing this field in a non-observable query has no impact.
     * <pre>
     * {@literal @}Dao
     * interface RawDao {
     *     {@literal @}RawQuery(observedEntities = User.class)
     *     LiveData&lt;List&lt;User>> getUsers(String query);
     * }
     * LiveData&lt;List&lt;User>> liveUsers = rawDao.getUsers("select * from User ORDER BY name
     * DESC");
     * </pre>
     *
     * @return List of entities that should invalidate the query if changed.
     */
    Class[] observedEntities() default {};
}
```

## Relation


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * A convenience annotation which can be used in a Pojo to automatically fetch relation entities.
 * When the Pojo is returned from a query, all of its relations are also fetched by Room.
 *
 * <pre>
 * {@literal @}Entity
 * public class Pet {
 *     {@literal @} PrimaryKey
 *     int id;
 *     int userId;
 *     String name;
 *     // other fields
 * }
 * public class UserNameAndAllPets {
 *   public int id;
 *   public String name;
 *   {@literal @}Relation(parentColumn = "id", entityColumn = "userId")
 *   public List&lt;Pet&gt; pets;
 * }
 *
 * {@literal @}Dao
 * public interface UserPetDao {
 *     {@literal @}Query("SELECT id, name from User")
 *     public List&lt;UserNameAndAllPets&gt; loadUserAndPets();
 * }
 * </pre>
 * <p>
 * The type of the field annotated with {@code Relation} must be a {@link java.util.List} or
 * {@link java.util.Set}. By default, the {@link Entity} type is inferred from the return type.
 * If you would like to return a different object, you can specify the {@link #entity()} property
 * in the annotation.
 * <pre>
 * public class User {
 *     int id;
 *     // other fields
 * }
 * public class PetNameAndId {
 *     int id;
 *     String name;
 * }
 * public class UserAllPets {
 *   {@literal @}Embedded
 *   public User user;
 *   {@literal @}Relation(parentColumn = "id", entityColumn = "userId", entity = Pet.class)
 *   public List&lt;PetNameAndId&gt; pets;
 * }
 * {@literal @}Dao
 * public interface UserPetDao {
 *     {@literal @}Query("SELECT * from User")
 *     public List&lt;UserAllPets&gt; loadUserAndPets();
 * }
 * </pre>
 * <p>
 * In the example above, {@code PetNameAndId} is a regular Pojo but all of fields are fetched
 * from the {@code entity} defined in the {@code @Relation} annotation (<i>Pet</i>).
 * {@code PetNameAndId} could also define its own relations all of which would also be fetched
 * automatically.
 * <p>
 * If you would like to specify which columns are fetched from the child {@link Entity}, you can
 * use {@link #projection()} property in the {@code Relation} annotation.
 * <pre>
 * public class UserAndAllPets {
 *   {@literal @}Embedded
 *   public User user;
 *   {@literal @}Relation(parentColumn = "id", entityColumn = "userId", entity = Pet.class,
 *           projection = {"name"})
 *   public List&lt;String&gt; petNames;
 * }
 * </pre>
 * <p>
 * Note that {@code @Relation} annotation can be used only in Pojo classes, an {@link Entity} class
 * cannot have relations. This is a design decision to avoid common pitfalls in {@link Entity}
 * setups. You can read more about it in the main Room documentation. When loading data, you can
 * simply work around this limitation by creating Pojo classes that extend the {@link Entity}.
 * <p>
 * Note that the {@code @Relation} annotated field cannot be a constructor parameter, it must be
 * public or have a public setter.
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.CLASS)
public @interface Relation {
    /**
     * The entity to fetch the item from. You don't need to set this if the entity matches the
     * type argument in the return type.
     *
     * @return The entity to fetch from. By default, inherited from the return type.
     */
    Class entity() default Object.class;
    /**
     * Reference field in the parent Pojo.
     * <p>
     * If you would like to access to a sub item of a {@link Embedded}d field, you can use
     * the {@code .} notation.
     * <p>
     * For instance, if you have a {@link Embedded}d field named {@code user} with a sub field
     * {@code id}, you can reference it via {@code user.id}.
     * <p>
     * This value will be matched against the value defined in {@link #entityColumn()}.
     *
     * @return The field reference in the parent object.
     */
    String parentColumn();
    /**
     * The field path to match in the {@link #entity()}. This value will be matched against the
     * value defined in {@link #parentColumn()}.
     */
    String entityColumn();
    /**
     * If sub fields should be fetched from the entity, you can specify them using this field.
     * <p>
     * By default, inferred from the the return type.
     *
     * @return The list of columns to be selected from the {@link #entity()}.
     */
    String[] projection() default {};
}
```

## RoomMasterTable


```java

package androidx.room;
import androidx.annotation.RestrictTo;
/**
 * Schema information about Room's master table.
 *
 * @hide
 */
@SuppressWarnings("WeakerAccess")
@RestrictTo(RestrictTo.Scope.LIBRARY_GROUP)
public class RoomMasterTable {
    /**
     * The master table where room keeps its metadata information.
     */
    public static final String TABLE_NAME = "room_master_table";
    // must match the runtime property Room#MASTER_TABLE_NAME
    public static final String NAME = "room_master_table";
    private static final String COLUMN_ID = "id";
    private static final String COLUMN_IDENTITY_HASH = "identity_hash";
    public static final String DEFAULT_ID = "42";
    public static final String CREATE_QUERY = "CREATE TABLE IF NOT EXISTS " + TABLE_NAME + " ("
            + COLUMN_ID + " INTEGER PRIMARY KEY,"
            + COLUMN_IDENTITY_HASH + " TEXT)";
    public static final String READ_QUERY = "SELECT " + COLUMN_IDENTITY_HASH
            + " FROM " + TABLE_NAME + " WHERE "
            + COLUMN_ID + " = " + DEFAULT_ID + " LIMIT 1";
    /**
     * We don't escape here since we know what we are passing.
     */
    public static String createInsertQuery(String hash) {
        return "INSERT OR REPLACE INTO " + TABLE_NAME + " ("
                + COLUMN_ID + "," + COLUMN_IDENTITY_HASH + ")"
                + " VALUES(" + DEFAULT_ID + ", \"" + hash + "\")";
    }
    private RoomMasterTable() {
    }
}
```

## RoomWarnings


```java

package androidx.room;
/**
 * The list of warnings that are produced by Room.
 * <p>
 * You can use these values inside a {@link SuppressWarnings} annotation to disable the warnings.
 */
@SuppressWarnings({"unused", "WeakerAccess"})
public class RoomWarnings {
    /**
     * The warning dispatched by Room when the return value of a {@link Query} method does not
     * exactly match the fields in the query result.
     */
    // if you change this, don't forget to change androidx.room.vo.Warning
    public static final String CURSOR_MISMATCH = "ROOM_CURSOR_MISMATCH";
    /**
     * Reported when Room cannot verify database queries during compilation due to lack of
     * tmp dir access in JVM.
     */
    public static final String MISSING_JAVA_TMP_DIR = "ROOM_MISSING_JAVA_TMP_DIR";
    /**
     * Reported when Room cannot verify database queries during compilation. This usually happens
     * when it cannot find the SQLite JDBC driver on the host machine.
     * <p>
     * Room can function without query verification but its functionality will be limited.
     */
    public static final String CANNOT_CREATE_VERIFICATION_DATABASE =
            "ROOM_CANNOT_CREATE_VERIFICATION_DATABASE";
    /**
     * Reported when an {@link Entity} field that is annotated with {@link Embedded} has a
     * sub field which is annotated with {@link PrimaryKey} but the {@link PrimaryKey} is dropped
     * while composing it into the parent object.
     */
    public static final String PRIMARY_KEY_FROM_EMBEDDED_IS_DROPPED =
            "ROOM_EMBEDDED_PRIMARY_KEY_IS_DROPPED";
    /**
     * Reported when an {@link Entity} field that is annotated with {@link Embedded} has a
     * sub field which has a {@link ColumnInfo} annotation with {@code index = true}.
     * <p>
     * You can re-define the index in the containing {@link Entity}.
     */
    public static final String INDEX_FROM_EMBEDDED_FIELD_IS_DROPPED =
            "ROOM_EMBEDDED_INDEX_IS_DROPPED";
    /**
     * Reported when an {@link Entity} that has a {@link Embedded}d field whose type is another
     * {@link Entity} and that {@link Entity} has some indices defined.
     * These indices will NOT be created in the containing {@link Entity}. If you want to preserve
     * them, you can re-define them in the containing {@link Entity}.
     */
    public static final String INDEX_FROM_EMBEDDED_ENTITY_IS_DROPPED =
            "ROOM_EMBEDDED_ENTITY_INDEX_IS_DROPPED";
    /**
     * Reported when an {@link Entity}'s parent declares an {@link Index}. Room does not
     * automatically inherit these indices to avoid hidden costs or unexpected constraints.
     * <p>
     * If you want your child class to have the indices of the parent, you must re-declare
     * them in the child class. Alternatively, you can set {@link Entity#inheritSuperIndices()}
     * to {@code true}.
     */
    public static final String INDEX_FROM_PARENT_IS_DROPPED =
            "ROOM_PARENT_INDEX_IS_DROPPED";
    /**
     * Reported when an {@link Entity} inherits a field from its super class and the field has a
     * {@link ColumnInfo} annotation with {@code index = true}.
     * <p>
     * These indices are dropped for the {@link Entity} and you would need to re-declare them if
     * you want to keep them. Alternatively, you can set {@link Entity#inheritSuperIndices()}
     * to {@code true}.
     */
    public static final String INDEX_FROM_PARENT_FIELD_IS_DROPPED =
            "ROOM_PARENT_FIELD_INDEX_IS_DROPPED";
    /**
     * Reported when a {@link Relation} {@link Entity}'s SQLite column type does not match the type
     * in the parent. Room will still do the matching using {@code String} representations.
     */
    public static final String RELATION_TYPE_MISMATCH = "ROOM_RELATION_TYPE_MISMATCH";
    /**
     * Reported when a `room.schemaLocation` argument is not provided into the annotation processor.
     * You can either set {@link Database#exportSchema()} to {@code false} or provide
     * `room.schemaLocation` to the annotation processor. You are strongly adviced to provide it
     * and also commit them into your version control system.
     */
    public static final String MISSING_SCHEMA_LOCATION = "ROOM_MISSING_SCHEMA_LOCATION";
    /**
     * When there is a foreign key from Entity A to Entity B, it is a good idea to index the
     * reference columns in B, otherwise, each modification on Entity A will trigger a full table
     * scan on Entity B.
     * <p>
     * If Room cannot find a proper index in the child entity (Entity B in this case), Room will
     * print this warning.
     */
    public static final String MISSING_INDEX_ON_FOREIGN_KEY_CHILD =
            "ROOM_MISSING_FOREIGN_KEY_CHILD_INDEX";
    /**
     * Reported when a Pojo has multiple constructors, one of which is a no-arg constructor. Room
     * will pick that one by default but will print this warning in case the constructor choice is
     * important. You can always guide Room to use the right constructor using the @Ignore
     * annotation.
     */
    public static final String DEFAULT_CONSTRUCTOR = "ROOM_DEFAULT_CONSTRUCTOR";
    /**
     * Reported when a @Query method returns a Pojo that has relations but the method is not
     * annotated with @Transaction. Relations are run as separate queries and if the query is not
     * run inside a transaction, it might return inconsistent results from the database.
     */
    public static final String RELATION_QUERY_WITHOUT_TRANSACTION =
            "ROOM_RELATION_QUERY_WITHOUT_TRANSACTION";
    /** @deprecated This type should not be instantiated as it contains only static methods. */
    @Deprecated
    @SuppressWarnings("PrivateConstructorForUtilityClass")
    public RoomWarnings() {
    }
}
```

## SkipQueryVerification


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Skips database verification for the annotated element.
 * <p>
 * If it is a class annotated with {@link Database}, none of the queries for the database will
 * be verified at compile time.
 * <p>
 * If it is a class annotated with {@link Dao}, none of the queries in the Dao class will
 * be verified at compile time.
 * <p>
 * If it is a method in a Dao class, just the method's sql verification will be skipped.
 * <p>
 * You should use this as the last resort if Room cannot properly understand your query and you are
 * 100% sure it works. Removing validation may limit the functionality of Room since it won't be
 * able to understand the query response.
 */
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.CLASS)
public @interface SkipQueryVerification {
}
```

## Transaction


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method in a {@link Dao} class as a transaction method.
 * <p>
 * When used on a non-abstract method of an abstract {@link Dao} class,
 * the derived implementation of the method will execute the super method in a database transaction.
 * All the parameters and return types are preserved. The transaction will be marked as successful
 * unless an exception is thrown in the method body.
 * <p>
 * Example:
 * <pre>
 * {@literal @}Dao
 * public abstract class ProductDao {
 *    {@literal @}Insert
 *     public abstract void insert(Product product);
 *    {@literal @}Delete
 *     public abstract void delete(Product product);
 *    {@literal @}Transaction
 *     public void insertAndDeleteInTransaction(Product newProduct, Product oldProduct) {
 *         // Anything inside this method runs in a single transaction.
 *         insert(newProduct);
 *         delete(oldProduct);
 *     }
 * }
 * </pre>
 * <p>
 * When used on a {@link Query} method that has a {@code Select} statement, the generated code for
 * the Query will be run in a transaction. There are 2 main cases where you may want to do that:
 * <ol>
 *     <li>If the result of the query is fairly big, it is better to run it inside a transaction
 *     to receive a consistent result. Otherwise, if the query result does not fit into a single
 *     {@link android.database.CursorWindow CursorWindow}, the query result may be corrupted due to
 *     changes in the database in between cursor window swaps.
 *     <li>If the result of the query is a Pojo with {@link Relation} fields, these fields are
 *     queried separately. To receive consistent results between these queries, you probably want
 *     to run them in a single transaction.
 * </ol>
 * Example:
 * <pre>
 * class ProductWithReviews extends Product {
 *     {@literal @}Relation(parentColumn = "id", entityColumn = "productId", entity = Review.class)
 *     public List&lt;Review> reviews;
 * }
 * {@literal @}Dao
 * public interface ProductDao {
 *     {@literal @}Transaction {@literal @}Query("SELECT * from products")
 *     public List&lt;ProductWithReviews> loadAll();
 * }
 * </pre>
 * If the query is an async query (e.g. returns a {@link androidx.lifecycle.LiveData LiveData}
 * or RxJava Flowable, the transaction is properly handled when the query is run, not when the
 * method is called.
 * <p>
 * Putting this annotation on an {@link Insert}, {@link Update} or {@link Delete} method has no
 * impact because they are always run inside a transaction. Similarly, if it is annotated with
 * {@link Query} but runs an update or delete statement, it is automatically wrapped in a
 * transaction.
 */
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.CLASS)
public @interface Transaction {
}
```

## TypeConverter


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Marks a method as a type converter. A class can have as many @TypeConverter methods as it needs.
 * <p>
 * Each converter method should receive 1 parameter and have non-void return type.
 *
 * <pre>
 * // example converter for java.util.Date
 * public static class Converters {
 *    {@literal @}TypeConverter
 *    public Date fromTimestamp(Long value) {
 *        return value == null ? null : new Date(value);
 *    }
 *
 *    {@literal @}TypeConverter
 *    public Long dateToTimestamp(Date date) {
 *        if (date == null) {
 *            return null;
 *        } else {
 *            return date.getTime();
 *        }
 *    }
 *}
 * </pre>
 * @see TypeConverters
 */
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.CLASS)
public @interface TypeConverter {
}
```

## TypeConverters


```java

package androidx.room;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * Specifies additional type converters that Room can use. The TypeConverter is added to the scope
 * of the element so if you put it on a class / interface, all methods / fields in that class will
 * be able to use the converters.
 * <ul>
 * <li>If you put it on a {@link Database}, all Daos and Entities in that database will be able to
 * use it.
 * <li>If you put it on a {@link Dao}, all methods in the Dao will be able to use it.
 * <li>If you put it on an {@link Entity}, all fields of the Entity will be able to use it.
 * <li>If you put it on a POJO, all fields of the POJO will be able to use it.
 * <li>If you put it on an {@link Entity} field, only that field will be able to use it.
 * <li>If you put it on a {@link Dao} method, all parameters of the method will be able to use it.
 * <li>If you put it on a {@link Dao} method parameter, just that field will be able to use it.
 * </ul>
 * @see TypeConverter
 */
@Target({ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.FIELD})
@Retention(RetentionPolicy.CLASS)
public @interface TypeConverters {
    /**
     * The list of type converter classes. If converter methods are not static, Room will create
     * an instance of these classes.
     *
     * @return The list of classes that contains the converter methods.
     */
    Class<?>[] value();
}
```

## Update


```java

package androidx.room;
/**
 * Marks a method in a {@link Dao} annotated class as an update method.
 * <p>
 * The implementation of the method will update its parameters in the database if they already
 * exists (checked by primary keys). If they don't already exists, this option will not change the
 * database.
 * <p>
 * All of the parameters of the Update method must either be classes annotated with {@link Entity}
 * or collections/array of it.
 *
 * @see Insert
 * @see Delete
 */
public @interface Update {
    /**
     * What to do if a conflict happens.
     * @see <a href="https://sqlite.org/lang_conflict.html">SQLite conflict documentation</a>
     *
     * @return How to handle conflicts. Defaults to {@link OnConflictStrategy#ABORT}.
     */
    @OnConflictStrategy
    int onConflict() default OnConflictStrategy.ABORT;
}
```


