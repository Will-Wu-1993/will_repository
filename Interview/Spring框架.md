## Spring框架
### Spring整体架构
#### Java领域第一框架
- Spring Framework
- Spring Boot
- Spring Cloud
常规所说的Spring框架就是Spring FrameWork，大约20个模块，主要包括：
#### 1、Core Container（核心容器）
1. Core
2. Beans
3. Context
4. Expression Language  

**SqEL**  
Core和Beans是框架的基础，提供了IoC和DI，以工厂模式的实现来完成配置和业务逻辑的解耦。  

Context基于Core和Beans提供了大量的扩展，包括国际化操作（基于JDK）、资源加载（基于JDK）、数据校验（Spring自己封装的数据校验机制）、数据绑定（Spring特有，HTTP请求中的参数直接映射称POJO）、类型转换，ApplicationContext接口是Context的核心。  
#### 2、AOP
讲面向切面编程直接集中到了Spring中，进行面向切面的业务开发，事务管理。
### 2、Spring IoC实现
前置知识：反射 + XML解析
1. IoC读取spring.xml获取bean相关信息，类信息，属性值信息（XML解析）
2. 根据第一步获取的信息，创建对象（运用反射）
    - 创建对象：通过反射机制获取到目标类的构造函数。
    - 给对象赋值
### 3、Spring AOP实现
JdkDynamicAOPProxy
- getProxy   返回动态代理对象
```java
return Proxy.newProxyInstance(classLoader, proxiedInterfaces, this);
```
- invoke   执行方法
程序是通过JdkDynamicAOPProxy类来动态创建一个代理类，进而动态创建一个代理bean。  

```java
Proxy.newProxyInstance(classLoader, proxiedInterfaces, this);
```
作用动态创建类，动态创建对象
- ClassLoader：类加载器
- Interfaces：目标类的接口get
- this

### 4、如何获取ApplicationContext
1. 使用@Autowired
```java
    @Autowired
    private ApplicationContext applicationContext;
```
2. 实现ApplicationContext接口