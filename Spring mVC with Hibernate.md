# Spring mVC with Hibernate

```xml

    <!-- Step 1: Define Database DataSource / connection pool -->
	<bean id="myDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
          destroy-method="close">
        <property name="driverClass" value="com.mysql.cj.jdbc.Driver" />
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/web_customer_tracker?useSSL=false&amp;serverTimezone=UTC" />
        <property name="user" value="springstudent" />
        <property name="password" value="springstudent" /> 

        <!-- these are connection pool properties for C3P0 -->
		<property name="initialPoolSize" value="5"/>
        <property name="minPoolSize" value="5" />
        <property name="maxPoolSize" value="20" />
        <property name="maxIdleTime" value="30000" />
	</bean>  
	
    <!-- Step 2: Setup Hibernate session factory -->
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<property name="dataSource" ref="myDataSource" />
		<property name="packagesToScan" value="com.luv2code.springdemo.entity,other packages" />
		<property name="hibernateProperties">
		   <props>
		      <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
		      <prop key="hibernate.show_sql">true</prop>
		   </props>
		</property>
   </bean>	  

    <!-- Step 3: Setup Hibernate transaction manager -->
	<bean id="myTransactionManager"
            class="org.springframework.orm.hibernate5.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>
    
    <!-- Step 4: Enable configuration of transactional behavior based on annotations -->
	<tx:annotation-driven transaction-manager="myTransactionManager" />

```

### DAO

- Define a package DAO , inside which create class and inteface

- In the class we need **to inject Spring Factory**

  ```java
  @Repository
  public classs DAO{
  
      @Autowired
      private SessionFactory sessionfactory;
  
      @Transactional
      public List<Customer> getCustomer(){
          Session session  = sessionFactory.getCurrentSession();
          Query<Customer> q = session.createQuery("from Customer",Customer.class);
          return q.list();
      }    
      
  }
  
  
  ```


- at repository coverts checked exception into uncheked;

- Inject DAO to the controller

  ```java
  public class Controller{
  	@Autowired
  	private DAO dao;
  
  }
  ```


### Service

- Can be used to collect data from differnet DAO 
- WE can extract form multiple DAO and then we can give it at once in cintroller
- We define iinterface and implementation

```java
@Service
public class Service{
	@AUtowired
	private DAO dao;
	
	@Transactional
	public List<Customer> getCustimer(){
		return dao.getCustomer();
	}
}
```

- Now just replce dao with service in controller



- #### Input button to navigate

  ```
  <input type="button" value="" onclick="window.location.href='showFormAdd'; return false;">
  ```

  

- #### Redirecting using Controller

```
 public controller{
 	return "redirect:/path"
 }
```

- #### Updating link

```xml
<% taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
//this is a customer URL with the id of the customer to be updated in as param
<c:url var="updateLink" value="/path">
	<c:param name="customerId" value="${cusomer.id}"    
</c:url>
    
    
 <form:hidden path="id" />
    
    
 session.save()
 session.update()
 session.saveOrUpdate()
```

- #### deleting link

  ```html
  <a href="${deleteLink}" onclick="if (!confirm('Are you sure sure?'))) returun false">Deleter</a>
  ```

  