# Spring MVC basics

#### In the JSP pages , refer param by

```
{params.name}
```

#### In the Controller , refer to the params

```java
public String test(HttpServletRequest req , Model model){
    String a = req.getParameter("naem");
    model.addAttribure("message",result);
}

use modeal valuw as {message} in the jsp

public String test(@RequestParam("anem") String sti , Model model){
    
}
```

#### Adding css / Static files

​	**add resources folder in root web directory**

```
do configuratopn in the spring.xml file, already weitten in xml file

In the view pages 
just use it as  ${pageContext.request.contextPath}/resources/-----
```





#### Root path of the web ${pageContext.request.contextPath}

#### We can alis add welcome file in the web.xml file

#### to redirect 

```
<% response.sendRedirect("customer/ads"); %>
```





### @Request Mapping

```
 @RequestMapping(path="/process" , method=RequestMethod.GET)
```



### @PathVariable

```
@GetMapping("/students/{studentId}")
public Student getStudent(@PathVariable int stduent){

}
```



#### Input button to navigate

```html
<input type="button" value="" onclick="window.location.href='showFormAdd'; return false;">

<a href="${deleteLink}" onclick="if (!confirm('Are you sure sure?'))) returun false">Deleter</a>



```

#### Redirecting using Controller

```
 public controller{
 	return "redirect:/path"
 }
```



```
<% taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>


<% taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
//this is a customer URL with the id of the customer to be updated in as param
<c:url var="updateLink" value="/path">
	<c:param name="customerId" value="${cusomer.id}"    
</c:url>
    
    
 <form:hidden path="id" />
    

```



### Using maven

- ### Maven dependenicies to add

  > spring-webmvc
  >
  > javax.swervlet-api
  >
  > javax.swervlet.jsp-api
  >
  > jstl

  ##### as we dont have web.xml for 	pure java config of the app , we need to add maven-war-lugin

  We are using pure java config for spring.xml and web.xml





