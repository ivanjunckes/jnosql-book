## Models Annotation

As mentioned previously, Artemis has annotations that make the Java developer life easier; this annotation has two categories:

* Annotations Models

* Qualifier annotation

#### Annotations Models

The annotation Models is to converts the entity model to the entity on communication, the Diana entity:

* Entity

* Column

* MappedSuperclass

* Key

##### Entity

This annotation maps the class to Artemis. It has a unique attribute: name. This attribute is to inform either the column family name or the document collection name, etc.. The default value is the simple name class, for example, given the org.jnosql.demo.Person class the default name will `Person`.

```java
@Entity
public class Person {
}
```

```java
@Entity("name")
public class Person {
}
```

##### Column

This annotation it to define which fields on an Entity will be persisted. It also has a unique attribute name to specify that name on Database, and the default value is the field name.

```java
@Entity
public class Person {
    @Column
    private String nickname;
    @Column
    private String name;
    @Column
    private List<String> phones;
    //ignored
    private String address;
//getter and setter
}
```

##### MappedSuperclass

If this annotation puts on a Parent class, the Artemis will persist its information as well. So beyond the son class, Artemis will store any field that is in Parent class with Column annotation.

##### Key

Just to Key-value database, that shows on the key-value database with a field is a key.

```java
@Entity
public class User implements Serializable {

    @Key
    private String userName;

    private String name;

    private List<String> phones;
    }
```

#### Qualifier annotation

That is important to work with more than one type of the same application.

```java
@Inject
private DocumentRepository repositoryA;
@Inject
private DocumentRepository repositoryB;
```

With the same two injections to the same interface, CDI throws an ambiguous exception. There is the `Database` qualifier to fixes problem like this. It has two attributes:

* **DatabaseType**: The database type, key-value, document, column, graph.

* **privider**: The provider database name, eg. "cassandra, "hbase", "mongodb". So using the `Database` qualifier:


```java
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseA”)
private DocumentRepository repositoryA;
@Inject
@Database(value = DatabaseType.DOCUMENT, provider = “databaseB”)
private DocumentRepository repositoryB;
```

Beyond this annotation, the producer method with the entity manager is required.

The benefits of use this qualifier instead of creating a new one. If the Manager Entity be produced using `Database` as a qualifier, Artemis will create classes such as DocumentRepository, ColumnRepository, etc. automatically.

