# Spring AOP

> Advice , Aspect,POintcut, JoinPoint
>
> We nedd AspejctJ jar file also
>
> ###### 	We need to add in the configuration file @EnableAutoJProxy

```java
@Configuraiton
@EnableAutoJProxy
@ComponentScan("basePackage") // base pacjages and aspects
public class DemoConfig{

}




//To create an aspect we do
@Aspect
@Component
public class MyLogginAspect{
    @Before("")
    public cvoid method(){
        
    }
}
```

### POINTCUT EXPRESSION

```
A predicate expressoion where adivce should be applied // Use ASpectJ pointcut expression language
```

- #### Execution pointcut == execute on execution

  **execution(modifier-pattern?** 

  **return-type-pattern** 

  **declaring-type-partter?** 

  **method-name-pattern(param-patter)**

  **throws-pattern?)**

  - ### For param pattern use full name only

  > - Declaring type is basically full class name 	
  >
  > - \* matches on everuyhting
  >
  > - 
  >
  > - Param Pattern
  >
  >   > ( ) , (*)--one of any type , (..)--zero or more of any type

- #### Pointcut Declaration

  ```java
  @Pointcut("executoon()")
  private void forDAOPackage(){}
  
  @Before("forDAOPackage()")
  ```

  **We can combine pointcut Expression using && , || , !**

  ```
  @Before("forDAOPackage() && forDAOadsasd()")
  ```

  - Setting up a seprate class for declaratio

    ```
    @Aspect
    public class pointcutdeclaration{
    	@Pointcut("executoon()")
    	private void forDAOPackage(){}
    }
    
    But then in the aspect where we gonna uuse it , we need to give the Full name for the aspect
    
    @Compoenent
    @Aspect
    public class an{
    	@Before("com.teejendra.forDAOPAckage()");
    }
    ```

- #### JoinPoints- Access the methd signature and params in a advice

  ```java
  @Before("...")
  public void before(JoinPoint join){
  	MethodSignature methodSig  = (MethodSignnature) join.getSignature();
      
      Object[] args = join.getArgs();
      for(Object temp : args){
          sop(temp);
      }
  }
  ```

  

- #### Aspect order in whick they are match

  - Solution is to place each advide in a seprate aspect then use **@Order(number)**

    ```
    @Aspect
    @Component
    @Order(2)
    public class ans(){
    
    }
    ```

    

- ### @Before

  - <u>Run before any  method call</u>

    ```
    @Before("execution(public void addAccount())")
    ```

- #### @AfterReturning

  We can change the data ,pre-porcess data

  ```
   @AfterReturnig(pointcut="",returning="result")
   public void methid(JoinPoint join , List<Account> result){}
  ```

- #### AfterThrowing

  We cantstop exception , we can only do something about it

  ```java
   @AfterThrowing(pointcut="",returning="result",throwing="theExc")
   public void methid(JoinPoint join , Throwable theexc){
  	 
   }
  ```

- #### @After

  rUN REGARDS OF THE success or failure , so *Immediately next after the method*

  Key here is **After run before throwing** , and **also before returning**

  ***No access to the Exception****

  ```
   @After("")
   public void methid(JoinPoint join){}
  ```

- #### @Around

  Befire and after the methid , instrumentation of code can be donw

  **Proceeding join poiint is handle to the target method , can be used to execute target method**

  - We can handle,swallow,stop,rethrow the exception

  ```java
   @Around("")
   public Object methid(ProceedingJoinPoint join) throws Throwable{
       
       long begin = System.vurrentTimeMillis();
       Object Result = join.proceed();
       
        long begin = System.vurrentTimeMillis();
   }
  
  TimeUnit.SECONDS.sleep(5);
  ```

  
       
  ```java
    @Around("")
   public Object methid(ProceedingJoinPoint join) throws Throwable{
   
   	long begin = System.vurrentTimeMillis();
       try{
  	 	Object Result = join.proceed();
       }catch(Exception e){
           result="I like to move it,move it"
               
               throw e;
       }
   	 long begin = System.vurrentTimeMillis();
    }
  ```
  

  