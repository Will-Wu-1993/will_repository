## Spring Boot
自动配置

Spring Boot可以自动读取各种配置文件，并将项目中需要用到的组件自动加载到IoC容器中。

##### Spring Boot组件包括两部分：
- 开发者自定义组件（Context、Service、Repository）
- 框架自带组件（AutoConfiguration。。。）

@SpringBootApplication注解是由3部分组成：

* @SpringBootConfiguration
* @EnableAutoConfiguration
* @ComponentScan

### @SpringBootConfiguration
@Configuration标注一个配置类（用Java类替代XML配置。）
```Java
@Configuration
public class AccountConfiguration {

    @Bean(name = "account")
    public Account getAccount(){
        return new Account("root","root");
    }

}
```
[Spring Boot自动配置原理](https://juejin.im/post/5db25dcd51882564430c392e) --作者：未读代码（掘金）
