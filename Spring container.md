these are some exercises at Spring Boot bootcamp and its introduce us to some annotations:
`@Bean`
`@Component`
`@Qualifier`
### Q1:
```Java
@SpringBootAplication
public class SpringPollApplication{

public static void main(String[] args){
SpringApplication.run(SpringPollApplication.class, args);
	}

@Bean
public String getMessage1(){
	System.out.println("hey from message1");
	return "1";
	}
}
```

Output:
```sh
hey from message1
```

The returned value will be stored in the Spring poll.
```java
return "1";
```


### Q2:
```Java
@SpringBootAplication
public class SpringPollApplication{

public static void main(String[] args){
SpringApplication.run(SpringPollApplication.class, args);
	}

@Bean
@Qualifier("1")
public String getMessage1(){
	System.out.println("hey from message1");
	return "1";
	}

@Bean
public String getMessage2(@Qualifier("1") String data){
	System.out.println("hey from message2");
	return data;
	}
}
```

Output:
```sh
hey from message1
hey from message2
```

the returned value of `getMessage2` which is:
```java
return data;
```

will equals the returned value of `getMessage1()` because the arguments of `getMessage2()` is using the `@Qualifier` annotation so the constructor will take the value of `getMessage1()` .

```java
public String getMessage2(@Qualifier("1") String data){
```

### Q3
```java
@Bean
@Qualifier("1")
public String getMessage1(){
	System.out.println("hey from message1");
	return "1";
	}

@Bean
@Qualifier("2")
public String getMessage2(@Qualifier("3") String data){
	System.out.println("hey from message2");
	return data;
	}

@Bean
@Qualifier("3")
public String getMessage2(){
	System.out.println("hey from message3");
	return "3";
	}
```

Output:
```sh
hey from message1
hey from message3
hey from message2
```

The arguments of  `getMessage2()` is using the `@Qualifier("3")` annotation which map to the returned value of `getMessage3()`.

```java
public String getMessage2(@Qualifier("3") String data){
```


so the value of `getMessage3()` will be printed first after that `getMessage2` will be printed.

### Q4
```java
@Bean
@Qualifier("1")
public String getMessage1(){
	System.out.println("hey from message1");
	return "1";
	}

@Bean
@Qualifier("2")
public String getMessage2(@Qualifier("3") String data){
	System.out.println("hey from message2");
	return data;
	}

@Bean
@Qualifier("3")
public String getMessage2(){
	System.out.println("hey from message3");
	return "3";
	}

}
```

```java
public class MainController{

String data;

public MainController(@Qualifier("1") String data){
		this.data = data;
		System.out.println("hey from Main controller");
	}
}
```

Output:
```sh
hey from message1
hey from Main controller
hey from message2
hey from message3
```

the `MainController` class constructor will take the returned data from `getMessage1()`  because we used `@Qualifier("1")` in the parameters of the constructor.

### Q5
```java
@Bean
@Qualifier("1")
public String getMessage1(MainController mainController){
	System.out.println("hey from message1");
	return "1";
	}

@Bean
@Qualifier("2")
public String getMessage2(@Qualifier("3") String data){
	System.out.println("hey from message2");
	return data;
	}

@Bean
@Qualifier("3")
public String getMessage2(){
	System.out.println("hey from message3");
	return "3";
	}
```


```java
public class MainController{

String data;

@Component
public MainController(@Qualifier("2") String data){
		this.data = data;
		System.out.println("hey from Main controller");
	}
}
```

Output:
```sh
hey from message3
hey from message2
hey from main controller
hey from message1
```

`MainController` depends on `getMessage2()` which is depends on `getMessage3()` 

so it prints `hey from message3` first, then `hey from message2` until `hey from message1` 


the results depends on the JDK version and the Spring boot version.