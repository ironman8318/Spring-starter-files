# Spring MVC Form

##### In the view , add this for the support of form tag

```jsp
<%@taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```

##### in the controller do

```java
public String show(Model model){
    model.addAttribute("student",new Student())//POJO for data binding
    return "asdasd";
}
```

##### in the view

```jsp
<form:form action="" modelAttribute="student">
	FirstName  : <form:input path="firstName">
</form:form>
    
so getting and setting is doneautomatically
```

##### after form submission in the process Controoler

```java
public String form(@ModelAttribute("student") Student theStudent)
```

##### Spting MVC select option from Java class without harcdoing in view

- In the POJO define a map with key and value prop;
- Define getter and setter

```JSP
<form:select path="country">
    <form:options items="${student.countryMap}"></form:options>//calling getter of map that provide the list
    
</form:select>
```

- Reading a prop file

  ```xml
  In the springMVC.xml
  <util:properties id="countryOptions" location="classpath:../countries.properties" /> 
  ```

  

  ```java
  Value("#{countryOptions}") 
  private Map<String, String> countryOptions;
  
  @RequestMapping("/showForm") 
  public String showForm(Model theModel) { 
      // add student object to the model 
      theModel.addAttribute("student", theStudent); 
      // add the country options to the model 
      theModel.addAttribute("theCountryOptions", countryOptions); 
  
      return "student-form"; 
  }
  ```



##### Spring MVC adding radio button ,loading from class

```xml
<form:radiobuttons path="favoriteLanguage" items="${student.favoriteLanguageOptions}"  />
```





##### Spring MVC checkboxes

```xml
<form:checkbox path="operatingSystgem" value="Linux">
```

- Do this for all checkbox along with same operatingSystem path , and operating System is a **String[]**



##### Looping  through a array of something

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<c:forEach var="temp" items="${student.operatingSystems}">
    <li>{temp}</li>
</c:forEach>
```

