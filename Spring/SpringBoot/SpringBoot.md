## 1,为什么SpringBoot

### 自动配置

假设你受命用Spring开发一个简单的HelloWorld  Web应用程序。你该做什么？我能想到一些基本的需要。

- 一个项目结构，其中有一个包含必要依赖的Maven或者Gradle构建文件，最起码要有Spring MVC和Servlet API这些依赖。
- 一个web.xml文件（或者一个WebApplicationInitializer实现），其中声明了Spring的DispatcherServlet。
- 一个启用了Spring MVC的Spring配置。
- 一个控制器类，以“Hello World”响应HTTP请求。
- 一个用于部署应用程序的Web应用服务器，比如Tomcat。

但是使用SpringBoot,那么仅需要引入spring-boot-starter-web依赖就可,各种xml文件,java配置以及tomcat均不需要,仅仅需要引入配置类就行了.

```Java
@Bean public JdbcTemplate jdbcTemplate(DataSource dataSource) {
    return new JdbcTemplate(dataSource); 
} 
```



### 起步依赖

Spring Boot通过起步依赖为项目的依赖管理提供帮助。起步依赖其实就是特殊的Maven依赖和Gradle依赖，利用了传递依赖解析，把常用库聚合在一起，组成了几个为特定功能而定制的依赖.

举个例子，假设你正在用Spring  MVC构造一个REST  API，并将JSON（JavaScript  Object  Notation）作为资源表述。此外，你还想运用遵循JSR-303规范的声明式校验，并使用嵌入式的Tomcat服务器来提供服务。要实现以上目标，你在Maven或Gradle里至少需要以下8个依赖：

- org.springframework:spring-core 
- org.springframework:spring-web 
- org.springframework:spring-webmvc 
- com.fasterxml.jackson.core:jackson-databind 
- org.hibernate:hibernate-validator 

- org.apache.tomcat.embed:tomcat-embed-core 
- org.apache.tomcat.embed:tomcat-embed-el 
- org.apache.tomcat.embed:tomcat-embed-logging-juli

不过，如果打算利用Spring  Boot的起步依赖，你只需添加Spring  Boot的Web起步依赖（org.springframework.boot:spring-boot-starter-web）①，仅此一个。它会根据依赖传递把其他所需依赖引入项目里，你都不用考虑它们

### 条件注解

条件注解是spring boot自动配置的核心,这是Spring 4.0引入的新特性。条件化配置允许配置满足条件时配置才生效。

如下案例,将Animal实例按条件放进spring容器

```java
public class Animal {
    private String name;
    private int age;
}

/**
 * 条件类,返回true时,放了@Conditional(MyCondition.class)的配置才生效
 */
public class MyCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return 1 > 2;
    }
}

/**
 * 配置类
 */
@Configuration
public class AnimalConfig {
    @Bean
    @Conditional(MyCondition.class)
    public Animal animal() {
        Animal animal = new Animal();
        animal.setAge(10);
        animal.setName("kitty");
        return animal;
    }
}

/**
 * springboot项目
 */
@SpringBootApplication
public class SpringBootDemoApplication {
    public static void main(String[] args) {
        ApplicationContext run = SpringApplication.run(SpringBootDemoApplication.class,args);
        Animal bean = run.getBean(Animal.class);
        System.out.println(bean);
    }
}
```

当MyCondition类结果为true时,最后才能获取到animal实例,反之会报 错org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'animal' available

也就是说animal实例在spring容器中不存在

自动配置中使用的条件化注解

条件化注解

| 注解                            | 描述                                                         | 处理类                    | 使用方法                                                     |
| ------------------------------- | ------------------------------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| @ConditionalOnBean              | 当容器中至少存在一个指定name或class的Bean时，进行实例化      | OnBeanCondition           | @ConditionalOnBean(CacheManager.class)                       |
| @ConditionalOnMissingBean       | 当容器中指定name或class的Bean都不存在时，进行实例化          | OnBeanCondition           | @ConditionalOnMissingBean(CacheManager.class)                |
| @ConditionalOnClass             | 当类路径下至少存在一个指定的class时，进行实例化              | OnClassCondition          | @ConditionalOnClass({Aspect.class, Advice.class })           |
| @ConditionalOnMissingClass      | 当容器中指定class都不存在时，进行实例化                      | OnClassCondition          | @ConditionalOnMissingClass(“org.thymeleaf.templatemode.TemplateMode”) |
| @ConditionalOnSingleCandidate   | 当指定的Bean在容器中只有一个，或者有多个但是指定了首选的Bean时触发实例化 | OnBeanCondition           | @ConditionalOnSingleCandidate(DataSource.class)              |
| @ConditionalOnProperty          | 当指定的属性有指定的值时进行实例化                           | OnPropertyCondition       | @ConditionalOnProperty(prefix = “spring.aop”, name = “auto”) |
| @ConditionalOnResource          | 当类路径下有指定的资源时触发实例化                           | OnResourceCondition       | @ConditionalOnResource(resources = “classpath:META-INF/build.properties”) |
| @ConditionalOnExpression        | 基于SpEL表达式的条件判断,当为true的时候进行实例化            | OnExpressionCondition     | @ConditionalOnExpression(“true”)                             |
| @ConditionalOnWebApplication    | 当项目是一个Web项目时进行实例化                              | OnWebApplicationCondition | @ConditionalOnWebApplication                                 |
| @ConditionalOnNotWebApplication | 当项目不是一个Web项目时进行实例化                            | OnWebApplicationCondition | @ConditionalOnNotWebApplication                              |
| @ConditionalOnJava              | 当JVM版本为指定的版本范围时触发实例化                        | OnJavaCondition           | @ConditionalOnJava(ConditionalOnJava.JavaVersion.EIGHT)      |
| @ConditionalOnJndi              | 在JNDI存在的条件下触发实例化                                 | OnJndiCondition           | @ConditionalOnJndi({ “java:comp/TransactionManager”})        |

