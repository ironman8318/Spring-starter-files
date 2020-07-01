# Spring Validation

#### Simple annotation

```java
@NotNull @Min @Max @Size @Pattern @Future/@Past
```

- In the Class

  ###### @NotNull,@Size

  ```java
  @NotNull(message="is Required")
  @Size(min=1,message="")
  private String name;
  ```

- In the view

  ```xml
  <form:input	/><form:errors path="lastName" cssClass="error"/>
  ```

- in the controller,process controller

  ```java
  public String processForm(@Valid @ModelAttribute("student") Student theStudent, BindingResult result ){
      if(result.hasErrors()){
          dothis();
      }else
          dothat();
  }
  ```

  

### Required Validation failed for the whitespaces

- In contriller do, add a preprocessor

  ```java
  @InitBinder
  public String initBinder(WebDataBinder dataBinder){
      StringTrimmerEditor a  = new StringTrimmerEditor(true);
      dataBinder.registerCustomEditor(String.class , stringTrimmerEditor);
  }
  ```

  

##### @Min,@Max,@Pattern

```java
@Min(value=0,message="")
@Max(value=0,message="")
@Pattern(regexp="",message="")

```





### We use int to validate , but get a ugly message

- Solution is to use Integer



### We give String to Integer , we get Ugly message

Solution , create a custom error message

Make a properties file in the **src/resources/{file}**,properties file and add  a custom error message like *typeMismatch.customer.freepasses=INVALID_NUMBER*

Now we need to tell Sptring about it

- in spring.xml file , add

  ```xml
  <bean id="messageSource" class="org.springframework.context.support.ResourceBundleMwssageSource">
      <!-- message.properties is the file-->
  	<property name="basenames" value="resources/message"></property>
  </bean>
  ```

#### BTS

- if we print the bindingResult error objec t, we will get the codes error , and we are simply overriding it

- ###### So basically overriding , that if this error occurs , what to print irrespective of the default eroor messafe

​	



### Custom Annotation

- First we create a custom Annotaion Class like @CourseCode,and a class that contains the logic of aour check

  ```java
  @Constraint(validatedBy=CourseCodeConstraintValidator.class)
  @Target({ElementType.METHOD, ElementType.FIELD})
  @Retention(RetentionPolicy.RUNTIME)
  public @inteface CourseCode{
      public String value() default "LUV";
      
      public String message() default "must starts with";
      
      public Class<?>[] groups() default {};
      
      public Class<? extends  Payload>[] payload() default {};
  
  }
  
  ```

- Creating the logic class for the annotaion

  ```java
  public class CourseCodeValidator implements ConstraintValidator<CourseCode,String>{
      private String coursePrefix;
      
      public void initialize(CourseCode course){
          coursePrefic = course.value();
      }
      public boolean isValid(String theCode , ConstraintValidatorContext context){
          boolean result;
          if(code != null){
              result = theCode.startsWith(coursePrefix);
          }else return true;
      }
  }
  ```

  