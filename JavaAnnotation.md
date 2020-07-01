

# Java Annotation CheatSheet

#### Bean Definition

```java
@Bean
	public Coach beanId(){
		return new Coach();
	}
```




```java
@Autowired
@Qualifier("randomFortuneService") // when used with constructors use it inside the argument
```



#### Reading Prop File

```Java
@PropertySource("classpath:sports.props");

@Value("${}")
```




#### LifeCycle methods

```java
@PostConstruct
@PreDestroy

```



#### ComponentScan

```
@ComponentScan("package")
```


