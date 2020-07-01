### Bean basic definition

```
<!--add xml config file in the root src; -->

<bean scope="" init-method="fucntionname" destroy-method="functionName" id="" class ="">
	<constructor-arg ref="id" /> //constructor injection
	<property name="nameofProperty" ref="id" /> //setter injection
	<property name="value" ref="id" /> 
</bean>
```



### Reading property file

```
<beans>
	<context:property-placeholder location="classpath:file.properties" />
</beans>
```

### Automatic Scanning of the Compoenents

```
<beans>
	<context:Component-scan base-package="" />
</beans>
```

