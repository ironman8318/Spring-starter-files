# SPRING REST



Jackson integration is taken care of all by itself, i.e. automatic conversion

> 100-199  Informational
>
> 200-299 Success
>
> 300-399 Redirection
>
> 400-499 Client error
>
> 500-599 Server error



### @RestController

- Handles REST requests and responses
- Spring REST will automaic do the data binding itself



Now just create the basic MVC project , no need to add Jackson anywhere , **Just make sure that jackson is in the classpath**



### @PathVariable

```
@GetMapping("/students/{studentId}")
public Student getStudent(@PathVariable int stduent){
	
}
```



## Exception Handling Process\

- Create a custom error class

  - the custom error  response  class will be sent back to client as JSON

    ```java
    private class StiudentErrorResponse{
    	private int status;
        private String mesafe;
        private long timestaml
            
    }
    ```

    

- ​     Create custom student exception

  ```java
  public class StudentNotFOundExceptin extends RuntimeException{
  	public StudentNotFoundExcetion(String msg){
          super(msg);
      }
      
      and other constructors
  }	
  ```

- Throw exception

- add @ExceptioinHandler in the controller

  - #####  HttpStatus.NOT_FOUND.value()

  - ##### HttpStatus.BAD_REQUEST.value()

  ```
  @ExceptionalHandler //catch studentNotFOund
  public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exc){
  	StudentErrorResponse error = new StudentErrorRespinse;
  	eroor.setStatus(HttpStatus.NOT_FOUND.value())
  	error,setMessage(exc.getMesage());
  	.......
  	
  	return new ResponseEntity<>(error,HTTPStatus,NOT_FOUND);
  }
  
  
  public ResponseEntity<StudentErrorResponse> handleException(Exception exc){
  	StudentErrorResponse error = new StudentErrorRespinse;
  	eroor.setStatus(HttpStatus.NOT_FOUND.value())
  	error,setMessage(exc.getMesage());
  	.......
  	
  	return new ResponseEntity<>(error,HTTPStatus,NOT_FOUND);
  }
  ```





- ### GLOBAL Exception Handler

- ## @ControllerAdvice

- - Pre process request to the controllers
  - Post process respinses to handle exception
  - Global exceptiion

  ```
  @ControllerAdvice
  public class StudentRestExceptionHandler{
  
  }
  ```

  