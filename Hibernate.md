### 

# Hibernate



### Testing out JDBC Connection

```java
String driver = "com.mysql.jdbc.Driver";
Class.forName(driver);

Connection myconn = DriverManager.getConnection(url,user,password);
```



## Initial setup without integrating to MVC or stuff

``` 
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>

    <session-factory>

        <!-- JDBC Database connection settings -->
        <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/hb_student_tracker?useSSL=false&amp;serverTimezone=UTC</property>
        <property name="connection.username">hbstudent</property>
        <property name="connection.password">hbstudent</property>

        <!-- JDBC connection pool settings ... using built-in test pool -->
        <property name="connection.pool_size">1</property>

        <!-- Select our SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>

        <!-- Echo the SQL to stdout -->
        <property name="show_sql">true</property>

		<!-- Set the current session context -->
		<property name="current_session_context_class">thread</property>
 
    </session-factory>

</hibernate-configuration>
```



### Setting up Tables as Java POJO

###### remmber to use standartd javax.persistence APi during importing the anotation

```java
@Entity
@Table(name="student")
public class Student{
    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;
}
```



### Using Hibernate to retreive/send object

```java
SessionFactory factory = new Configuration().configure("hibernate,xml").addAnnotatedClass(Student.class).buildSessionFactory();

Session session = factory.getCurrentSession();

session.beginTransaction();
session.save(a);
    
 session.save()
 session.update()
 session.saveOrUpdate()
session.getTransaction().commit();


//Retreiving from database
Student a = session.get(Student.class,Id)
```

### HQL common queries for retreivinf

```java
List<Students> theStudents = session.createQuery("from student").list();

List<Students> theStudents = session.createQuery("from student s where s.lastName="Something\" OR s.lastName='AMAN'").list();

List<Students> theStudents = session.createQuery("from student s where s.email  LIKE '%luv2Code.com'").list();
```

### Updating Objects

```java
Student s = session.get(Student.class,Id);
s.setLastName("asdas");
session.getTransaction.commit();


```

### HQL common queries for updating

```java
session.createQuery("update Student set email='foo.email'").executeUpdate();
```



### Deleting Objects

```java
Student s = session.get(Student.class,Id);
session.delete(s);
session.getTransaction.commit();


```

### HQL common queries for deleting

```java
session.createQuery("delete from student where id=2").executeUpdate();
```





### Eaager and Lay Loading

```
@OneToOne(fetch=FetchType.LAZY)
```

- Session need to be opened if we Lazy Load, what if not solution

  - ```java
    public class Main{
    	//we getched the instrcitors lazy
        temp=session.get(Instructpr,2);
        course = tenp.getCourses()
        
        session.getTransaction().commit();
        session.close();
        course = tenp.getCourses() // works as we have already loaded course in mementor bu calling the method above
        
    }
    ```

  - ```java
    session.beginTranscation();
    Query<Instructor> query = session.createQuery("select i from Instructor i JOIN FETCH i.courses where i.id=:theinstructorId,",Instructor.class);
    
    query,setParameter("theInstructorId",theId);
    
    Instrcutor temp = query.getSingleResult();
    
    session.close();
        course = tenp.getCourses()
    
    ```

    

# Hibernate Mapping

- ## OneToOneMapping

  - Instructor -------------------------------> instructorDetail
  	- ```java
    public class Instructor{
     	@OneToOne(cascade=CascadeType.ALL)
     	@JoinColumn(name="instrutor_detail_id")
     	private InstructorDetail instructorDetail;
     }
  	  
  	  public class InstructorDetail{
  	  
  	   }
  	  
  	  
  	  //in main  App
  	  create Instcutor , InstructorDetail;
  	  Instructor.setInstructorDetail();
  	  Instructor.save();
  	  session.delet(instrictor)
  	  //instructor, Instrictor detail npth got saved;
  	      
  	  //save one other ger saved also
  	  ```




  - Instructor <-------------------------------> instructorDetail

    - ```java
      public class Instructor{
       	@OneToOne(cascade=CascadeType.ALL)
       	@JoinColumn(name="instrutor_detail_id")
       	private InstructorDetail instructorDetail;
       }
      
      public class InstructorDetail{
          @OneToOne(mappedBy="instructorDetail",cascade={CascadeType.DETACH,CascadeType.DELETE,CascadeType.MERGE,CascadeType.REFRESH,CascadeType.MERGE,CascadeType.ALL,}) //instructorDetail of the Instructor class
      	private Instructor instructor
       }
      
      
      //in main  App
      create Instcutor , InstructorDetail;
      Instructor.setInstructorDetail();
      Instructor.save();
      session.delet(instrictor)
      //instructor, Instrictor detail npth got saved;
          
      //save one other ger saved also
      ```

    -  deleting the assciated withoput deleting the main , require you to first delete the bordirectional link from associated to main;

  

- ### OneToMany - Bi

  ​	Instructor <---------------->Course,Course(F)	

  ​	Remember to set up the bidirectionLink

  ```java
  public class Instructor{
   	@OneToMany(mappedBy="instructor")
   	private List<Course> courses;
   }
  
  public class InstructorDetail{
  	@ManyToOne(cascade=CascadeType.ALL)
   	@JoinColumn(name="instructor_id",cascade={except delete})
   	private Instructor instructor;
   }
  
  
  //in main  App
  create Instcutor , InstructorDetail;
  Instructor.setInstructorDetail();
  Instructor.save();
  session.delet(instrictor)
  //instructor, Instrictor detail npth got saved;
      
  //save one other ger saved also
  ```

-  ### OneToMany-Uni

   ​	COurse ------------------>Review,Review(F)

   > Note here we have Review and Courses ,
   >
   > Review Have a foreigh key for the course
   >
   > Now in review we dont setup for the course_id anything at all
   >
   > In course we do it actially ...

  

```java
public class Course{
    
    @OneToMany(cascade=CascadeType,ALL)
    @JoinColumn(name="course_id")//this is actually in review table
    private List<Review> courseId;	
        
}
```

  

- ### ManyToMany

    Here we are going to use Join Table

  Courses ------------- Students

  ```java
  public class Course{
      @ManyToMany
      @JoinTable(name="course_student" joinColumns=@JoinColumn(name="course_id") inverseJoinColumns=@JoinColumn(name="student_id"))
      private List<Student> students;
  }
  
  public class Student{
      @ManyToMany
      @JoinTable(name="course_student" joinColumns=@JoinColumn(name="student_id") inverseJoinColumns=@JoinColumn(name="course_id"))
      private List<Course> courses;
  }
  ```

  > In the join table , we need to simply fill courses with student...No bi relation setup and shit

  

  

  

  

  

  

  


  ​    


​    




