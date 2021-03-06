## @Component vs @Bean

####  @Component: preferable for component scanning and automatic wiring.

#### When should you use @Bean?
Sometimes automatic configuration is not an option. When? Let's imagine that you want to wire components from 3rd-party libraries (you don't have the source code so you can't annotate its classes with @Component), so automatic configuration is not possible.
The @Bean annotation returns an object that spring should register as bean in application context. The body of the method bears the logic responsible for creating the instance.

## 1 Autowiring by @Component
**Step- 1: Create interface**
```
public interface IRepo{
  public int getRandomInt();
}
```
**Step - 2: Create class & implements that interfcae. Annotate class with @Component / @Service / @Repository**
```
@Repository
public class Repo implements IRepo {

  @Overrride
  public int getRandomInt(){
  
  }
}
```
**Step - 3: Annotate interface type reference variable with @Autowired**
```
@Controller
public class MyController {

  @Autowired
  private IRepo repo;
  
}
```
## 2 Autowiring by @Configuration & @Bean
If you want to customize the class instance registering as bean in application context (for DI) use @Configuration & @Bean annotations    

**Step - 1: Create class (don't mark with @Component / @Repository / @Service)**
```
public class SomeClass {

    private int theNumber;

    public SomeClass(Integer theNumber){
        this.theNumber = theNumber.intValue();
    }

}
```
**Step - 2: Create configuration class and use @Bean**
```
@Configuration
public class SomeConfig{ // SomeClass will be registered in application Context for DI

  @Bean
  public Integer generateNumber(){
      return new Integer(3456);
  }

  @Bean
  public SomeClass someClass(Integer theNumber){
      return new SomeClass(theNumber);
  }
}
```
**Step - 3: Use @Autowired**
```
@Controller
public class MyController {

  @Autowired
  private SomeClass someClass;
  
}
```

## @Bean only
Let's consider I want specific implementation depending on some dynamic state. @Bean is perfect for that case
```
@Bean
@Scope("prototype")
public SomeService someService() {
    switch (state) {
    case 1:
        return new Impl1();
    case 2:
        return new Impl2();
    case 3:
        return new Impl3();
    default:
        return new Impl();
    }
}
```
However there is no way to do that with @Component.

## Reasons for @Autowired component is null
1. YOU INSTANTIATED THE A CLASS MANUALLY
2. YOU FORGOT TO ANNOTATE A CLASS AS A COMPONENT OR ONE OF ITS DESCENDANTS

Details: [Two reasons why your Spring @Autowired component is null](https://www.moreofless.co.uk/spring-mvc-java-autowired-component-null-repository-service/)

