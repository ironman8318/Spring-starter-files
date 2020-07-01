# Jackson project

Handle all the getter and setter method when converting POJO to JSON or vice vesa

```java
ObjectMapper mapper = new ObjectMapper();

Student mystudent = mapper.readValue(new File("data/sample.json"),Student.class);


mapper.enable(SerializationFeature.INDENT_UPTPUT);
mapper.writeValue(new File("data/sample.json"),myStudent);


@JsonIgnoreProperties(ignoreUnknown=true)
public class Student{
	//add default constrictor
}
```

