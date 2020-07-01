# Spring security

### Maven dependenicies to add

> spring-webmvc
>
> javax.swervlet-api
>
> javax.swervlet.jsp-api
>
> jstl

##### as we dont have web.xml for 	pure java config of the app , we need to add maven-war-lugin

We are using pure java config for spring.xml and web.xml





## Spring securoty and mvc are different projects so , use incompatible version

- Go to Yoour springsecuroty and check its dependency for the compatible cersion of the spring mvc
- We need to add **Spring-security-web , spring-seucorty-web**



### we need to create the Spring security initailsizer nad spring secuirty configuration

- ```
  
  public class SecurityWebApplicationInitializer 
  						extends AbstractSecurityWebApplicationInitializer {
  
  }
  ```

  ```java
  
  @Configuration
  @EnableWebSecurity
  public class DemoSecurityConfig extends WebSecurityConfigurerAdapter {
  
  	@Override
  	protected void configure(AuthenticationManagerBuilder auth) throws Exception {
  
  		// add our users for in memory authentication
  		
  		UserBuilder users = User.withDefaultPasswordEncoder();
  		
  		auth.inMemoryAuthentication()
  			.withUser(users.username("john").password("test123").roles("EMPLOYEE"))
  			.withUser(users.username("mary").password("test123").roles("MANAGER","MANAGER",))
  			.withUser(users.username("susan").password("test123").roles("ADMIN"));
  	}
      
      
      @Override
  	protected void configure(HttpSecurity http) throws Exception {
  
  		http.authorizeRequests()
  				.anyRequest().authenticated()
  			.and()
  			.formLogin()
  				.loginPage("/showMyLoginPage")
  				.loginProcessingUrl("/authenticateTheUser") // no need for this controller to be made , handled automatically by the Spring
  				.permitAll().and().logout().permitAll(); // default path for logout is/logout
  		
  	}
  	
  }
  
  
  
  ```

  



#### Remeber to use *username , password* as name f the form fields 

### Adding custom error messages

> When the auth fails , spring redirects to the login form and append a error param at the end, which we can use to display eroor messge

``` 
<c : if test="${param.error != null}">	
</c:if>

<c : if test="${param.logout != null}">	
</c:if>
```





### Spring provide custom tags for accessing the userid and roles

- Update POM file to get ,**Spring Securoty JSP tag librabry------spring-securoty-taglibs**

- 

  ```html
  <%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>
      
      
   <security:authentication property="principal.username"></security:authentication>
       <security:authentication property="principal.authorities"></security:authentication>//we can give more than 1 roles
  ```



### Restricting access based on the role

In the securoty configure file do

```java
///**------------any sub path also
http.authorizeRequests()
.antMatchers("/").hasRole("Managet")
.antMatchers("/**").hasRole("Managet","Employee")
.and().formLogin()
```

### Access Denied Page

```
.exceptionHandling().accessDeniedPage("/access-denied") //auth error or cant access the page
```

### Displaying Contetn based on the roles

Will not be even inculdede in the system

```html
<security:authorize access="hasRole('MANAGER')">
	<p>
        
    </p>

</security:authorize>
```



## Database support in Spring Secuiry	

- Can read info from database , **But we need to follow the predefined table Schemas**
- We don't have to do any other thing, Spring will do it by itself

Users                                                                   authorities

username var(50)                                              username var50

password var(50)												authority var50 // Remeber to prefix as ROLE_EMPLOYEE

enabled TINYINT(1) // wheather user can login or not



##### Password are stored in a specific format

- ​	{algo}encodedPassword
  - noop - - plain text
  - bcrypt -- bcrypt



Remember to add mysql and c3P0 // connection pool

Then create the JDBC properties file /src/main/resources

```
#
# JDBC connection properties
#
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_security_demo_plaintext?useSSL=false
jdbc.user=springstudent
jdbc.password=springstudent

#
# Connection pool properties
#
connection.pool.initialPoolSize=5
connection.pool.minPoolSize=5
connection.pool.maxPoolSize=20
connection.pool.maxIdleTime=3000
```

then in springConfig.java

```

@Configuration
@EnableWebMvc
@ComponentScan(basePackages="com.luv2code.springsecurity.demo")
@PropertySource("classpath:persistence-mysql.properties")
public class DemoAppConfig {

	// set up variable to hold the properties
	
	@Autowired
	private Environment env;
	
	// set up a logger for diagnostics
	
	private Logger logger = Logger.getLogger(getClass().getName());
	
	
	// define a bean for ViewResolver

	@Bean
	public ViewResolver viewResolver() {
		
		InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
		
		viewResolver.setPrefix("/WEB-INF/view/");
		viewResolver.setSuffix(".jsp");
		
		return viewResolver;
	}
	
	// define a bean for our security datasource
	
	@Bean
	public DataSource securityDataSource() {
		
		// create connection pool
		ComboPooledDataSource securityDataSource
									= new ComboPooledDataSource();
				
		// set the jdbc driver class
		
		try {
			securityDataSource.setDriverClass(env.getProperty("jdbc.driver"));
		} catch (PropertyVetoException exc) {
			throw new RuntimeException(exc);
		}
		
		// log the connection props
		// for sanity's sake, log this info
		// just to make sure we are REALLY reading data from properties file
		
		logger.info(">>> jdbc.url=" + env.getProperty("jdbc.url"));
		logger.info(">>> jdbc.user=" + env.getProperty("jdbc.user"));
		
		
		// set database connection props
		
		securityDataSource.setJdbcUrl(env.getProperty("jdbc.url"));
		securityDataSource.setUser(env.getProperty("jdbc.user"));
		securityDataSource.setPassword(env.getProperty("jdbc.password"));
		
		// set connection pool props
		
		securityDataSource.setInitialPoolSize(
				getIntProperty("connection.pool.initialPoolSize"));

		securityDataSource.setMinPoolSize(
				getIntProperty("connection.pool.minPoolSize"));

		securityDataSource.setMaxPoolSize(
				getIntProperty("connection.pool.maxPoolSize"));

		securityDataSource.setMaxIdleTime(
				getIntProperty("connection.pool.maxIdleTime"));
		
		return securityDataSource;
	}
	
	// need a helper method 
	// read environment property and convert to int
	
	private int getIntProperty(String propName) {
		
		String propVal = env.getProperty(propName);
		
		// now convert to int
		int intPropVal = Integer.parseInt(propVal);
		
		return intPropVal;
	}
}

```



#### Now after defining all this shit , in the secrity config file do

```
@Configuration
@EnableWebSecurity
public class-----{
	@Autowired
	private DataSource dataSources;
	
	in the comfigure method
	auth.jdbcAuthentication().dataSource(dataSources)
}
```





Bcrypt passwor d is always 68 length wide