# Spring Boot

## 一、Spring Boot入门

### 1、简介

> Spring Boot 是用来**简化 Spring 的应用开发**，约定大于配置，产品级别的应用

- 背景：

  > J2EE 笨重的开发，繁多的配置、底下的开发效率、复杂的部署流程、第三方技术集成难度大

- 解决方式：

  > “Spring全家桶”
  > Spring Boot ---> **J2EE一站式解决方案**
  > Spring Cloud ---> 分布式整体解决方案

- 优点：

  > 快速创建独立运行的 Spring 项目以及与主流框架集成
  > 使用嵌入式的 Servlet 容器，应用无需打成 war 包
  > **starters 自动依赖与版本控制**
  > 大量的自动配置，简化开发，也可修改默认值
  > **无需配置 XML，无代码生成，开箱即用**
  > 准生产环境的运行时应用监控
  > 与云计算的天然集成

### 2、微服务
> 每一个功能元素最终都是一个可独立替换和独立升级的软件单元

<img src="images\1612496080652.png" alt="1612496080652" style="zoom:67%;" />

### 3、Spring Boot HelloWorld

① 创建一个 Maven 项目，导入 Spring boot 相关依赖（pom）：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.1</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

② 编写一个主程序类，启动 Spring boot 应用：

```java
@SpringBootApplication
public class HelloworldMainApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloworldMainApplication.class,args);
    }
}
```

#### @SpringBootApplication

> 用来标注一个主程序类，说明这是一个 Spring Boot 应用

③ 编写 controller/service

```java
@Controller
public class HelloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String hello(){
        return "hello world";
    }
}
```

④ 简化部署

> 打包（加入 pom 依赖）：
>
> - 使用 java `-jar 对应的jar包` 命令启动项目

```xml
<build>
    <plugins>
        <!-- 这个插件，可以将应用打包成一个可执行的 jar 包 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 4、Hello World 探究

#### pom 文件：

```xml
<!--父项目：-->
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.4.1</version>
</parent>
<!--他的父项目是：-->
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-dependencies</artifactId>
	<version>2.4.1</version>
</parent>
```

##### Spring Boot 的版本仲裁中心

> 真正管理 Spring boot 应用里的所有依赖版本
>
> 导入依赖默认不需要写版本号（没有在 dependencies 里面管理的依赖自然需要声明版本号）

```xml
<!--导入的依赖：-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

##### [starters](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#using-boot-starter)（启动器）:

> spring-boot-starter-web：
> 				spring-boot 的场景启动器，导入了 web 模块正常运行所依赖的组件
> 				Spring Boot 将所有的功能场景都抽取出来，做成  （启动器）

#### 主程序类：

```java
@SpringBootApplication
public class HelloworldMainApplication {
    public static void main(String[] args) {
		//	传入的第一个参数必须是被 @SpringBootApplication 标注的类，第二个参数为 main 方法上的参数
        SpringApplication.run(HelloworldMainApplication.class,args);
    }
}
```

> @SpringBootApplication：SpringBoot 应用标注在某个类上说明这个类是 SpringBoot 的主配置类
> SpringBoot 就应该运行这个类的 main 方法来启动 SpringBoot 应用

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
		@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {}
```

##### @SpringBootConfiguration

>Spring Boot 的配置类
>标注在某个类上，表示这是一个 Spring Boot 的配置类

###### @Configuration

> 配置类上来标注这个注解：配置类--->配置文件
>
> - @Component：配置类容器中的一个组件

##### @EnableAutoConfiguration

> 开启自动配置功能

###### @AutoConfigurationPackage（自动配置包）

```java
//将主配置类（@SpringBootApplication 标注的类）的所在包及下面所有子包里面的所有组件扫描到 Spring 容器
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

### 5、使用 Spring Initializer 快速创建 Spring Boot 项目

- 默认生成的 Spring Boot 项目：

  > 主程序已经生成，只需要编写自己的代码逻辑
  >
  > - resources 文件夹中的目录结构：
  >   - static：保存所有的静态资源（js，css，images）
  >   - templates：保存所有的模板页面（Spring Boot 默认 jar 包使用嵌入式的 Tomcat，默认不支持 JSP 页面），可以使用模板引擎（freemarker、thymeleaf）
  >   - application.properties：Spring Boot 应用的配置文件，可以修改一些默认设置（如：端口）

## 二、Spring Boot配置文件

### 1、配置文件：

> - 配置文件：Spring Boot 使用一个全局的配置文件，配置文件名是固定的
>   - application.properties
>   - application.yml
> - 配置文件的作用：修改 Spring Boot 自动配置的默认值，Spring Boot 在底层都已经自动配置好

### 2、YAML（YAML Ain't Markup Language）语法：

#### 简介：

> 以数据为中心，比 json、xml 更适合做配置文件

```yml
#例如：
server:
    port:8081
```

#### YAML 基本语法：

> - k:[空格] v: 
>   - 表示一对键值对（空格必须有）
>   - 以空格缩进来控制层级关系，只要是左对齐的一列数据，都是同一个层级的
>   - 属性和值都是大小写敏感
> - 注意：
>   - 使用缩进表示层级关系
>   - 缩进时不允许使用 Tab 键，只允许使用空格
>   - 缩进的空格数目不重要，只要相同层级的元素左侧对其即可
>   - 大小写敏感

#### YAML 支持的三种数据结构：

- 字面量：单个的、不可再分的值（数字、字符串、布尔值）

  >k: v: 字符串默认不加上单引号或者双引号
  >		""：双引号
  >			不会转义字符串里面的特殊字符，特殊字符会作为本身想表达的意思
  >		' '：单引号
  >			会转义特殊字符，特殊字符最终只是一个普通的字符数据

- 对象、Map(key,value)：键值对的集合

  >```yml
  ># k: v: 在下一行写对象的属性和值的关系，注意缩进
  >friends:
  >    lastName: zhang
  ># 行内写法：
  >fridens: {lastName: zhang,......}
  >```

- 数组：一组按次序排序的值

  > ```yml
  > # 用 “- 值”表示数组中的一个元素
  > pets:
  >     - cat
  > # 行内写法：
  > pets: [cat, dog, ...]
  > ```

### 3、获取配置文件：

#### @ConfigurationProperties：

>  告诉 SpringBoot 将本类中的所有属性和配置文件中相关的配置进行绑定（支持松散绑定）
>
>  - 用法：@ConfigurationProperties(prefix = “person”)
>  - 只有这个组件是容器中的组件，容器才能提供@ConfigurationProperties的功能

#### 步骤：

##### 1、编写 yml 文件：
```yml
person:
lastName: zhangxinrun
map: {k1: v1,k2: v2}
lists:
  - lisi
dog:
  name: 小狗
  age: 3
```
#####  2、给 Java Bean 赋值（将配置文件中配置的每一个属性的值，映射到这个组件中）：

```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

##### 3、在pom文件中加入如下内容

```xml
<dependency>
    <groupId>org.springframeworkl.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependenzcy>
```

#### @Value：
> 逐个指定给属性赋值

### 4、配置文件值注入：

> - @Value 和@ConfigurationProperties 获取值比较：
>   - @ConfigurationProperties：支持批量注入配置文件中的属性，支持松散绑定，不支持 SpEl 表达式，支持 JSR303 数据校验
>   - @Value：支持一个个指定，不支持松散绑定，支持 SpEl 表达式，不支持 JSR303 数据校验

### 5、JSR303 数据校验：

> 在类上面加入 @Validated 注解







配置项目的访问路径

server.context-path=/路径



### 外部















## 三、Spring Boot与日志

### 1、日志框架

> 市面上的日志框架：JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j...

| 日志门面（日志的抽象层）                                     | 日志实现                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| JCL（Jakarta Commons Logging）、<br />==SLF4j（Simple Logging Facade for Java）==、<br />jboss-logging | Log4j<br/>JUL（java.util.logging）<br/>Log4j2<br/>==Logback== |

> SpringBoot：
>
> - 底层是Spring框架，Spring框架默认是用JCL
>
> - **SpringBoot选用 SLF4j和logback**

### 2、SLF4j使用

#### 1）如何在系统中使用[SLF4j](https://www.slf4j.org)

> 日志记录方法的调用，不应该来直接调用日志的实现类，而是**调用日志抽象层里面的方法**

```java
// 给系统里面导入slf4j的jar和 logback的实现jar
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class HelloWorld {
    public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
    }
}
```

图示：

<img src="images\1612667909890.png" alt="1612667909890" style="zoom: 50%;" />

> 每一个日志的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架自己本身的配置文件**

#### 2）遗留问题

> Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis等统一日志记录，即使是别的框架一起统一使用，slf4j也能进行输出？

<img src="images\1612668463151.png" alt="1612668463151" style="zoom:67%;" />

#### 3）如何让系统中所有的日志都统一到slf4j

- 将系统中其他日志框架先排除出去
- 用中间包来替换原有的日志框架
- 导入slf4j其他的实现

### 3、Spring Boot日志关系

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter</artifactId>
</dependency>
```

```xml
<!--SpringBoot使用它来做日志功能；-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐logging</artifactId>
</dependency>
```

<img src="images\1612669679755.png" alt="1612669679755" style="zoom: 67%;" />



















































