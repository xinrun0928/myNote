# SpringMVC

## 一、SpringMVC 概述

### 1、SpringMVC概述

- Spring为展现层提供的基于MVC设计理念的优秀的Web框架，是目前最主流的MVC框架之一。
- Spring3.0后全面超越Struts2，成为最优秀的MVC框架。
- SpringMVC通过一套MVC注解，让POJO成为处理请求的控制器，而无需实现任何接口。
- 支持REST（Restful）风格的URL请求。
- 采用了松散耦合可插拔组件结构，比其他MVC框架更具扩展性和灵活性。

### 2、SpringMVC是什么

- 一种轻量级的、基于MVC的Web层应用框架，是Spring的后续版本。
- Spring框架结构图（新版本）：

### 3、SpringMVC能干什么

- Spring框架集成

- 支持Restful风格

- 进行更简洁的Web层开发

- 支持灵活的URL到页面控制器的映射
- 非常容易与其他视图技术集成，如：Velocity、FreeMarker等等
- 对静态资源的支持

### 4、SpringMVC怎么玩

1. 将Web层进行了职责解耦，基于请求--响应模型

2. 常用主要组件

   ① DispatcherServlet：前端控制器

   ② Controller：处理器/页面控制器，用于对请求的处理

   ③ HandlerMapping：请求映射到处理器

   ④ View Resolver：视图解析器，把逻辑视图解析为具体的View，进行这种策略模式，很容易更换其他视图技术

   - 如：InternalResourceViewResolver：将逻辑视图名映射为JSP视图

   ⑤ LocalResolver：本地化、国际化

   ⑥ MultipartResolver：文件上传解析器

   ⑦ HandlerExceptionResolver：异常处理器

### 5、SpringMVC中的HelloWorld

- 新建Web工程，导入jar包

  > spring-aop-5.2.6.RELEASE.jar
  >
  > spring-beans-5.2.6.RELEASE.jar
  >
  > spring-context-5.2.6.RELEASE.jar
  >
  > spring-core-5.2.6.RELEASE.jar
  >
  > spring-expression-5.2.6.RELEASE.jar
  >
  > commons-logging-1.1.1.jar
  >
  > spring-web-5.2.6.RELEASE.jar
  >
  > spring-webmvc-5.2.6.RELEASE.jar

- 在web.xml中配置SpringMVC的核心（前端）控制器---> 

  > org.springframework.web.servlet.DispatcherServlet

  作用：1、加载SpringMVC的配置文件，在下面的配置方式中，DispatcherServlet自动加载配置文件，此时的配置文件有默认的位置和名称。

  ​			2、默认位置：WEB-INF目录下，默认名称：\<servlet-name\>-servlet.xml，例如以下的配置方式的文件名：springMVC-servlet.xml。

  ​			3、当加载了配置文件，SpringMVC会根据扫描组件找到控制层。
  
  ```xml
  <!-- web.xml -->
  <servlet>
      <servlet-name>springMVC</servlet-name>
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  </servlet>
  <servlet-mapping>
  	<servlet-name>springMVC</servlet-name>
      <!--/表示根目录下的所有路径，/*表示根路径下的所有文件都被映射到（包括静态资源等）-->
      <url-pattern>/</url-pattern>
  </servlet-mapping>
  ```
  
  ```xml
  <!-- springMVC-servlet.xml -->
  <!-- 声明组件扫描器，将加上@Controller注解的类作为SpringMVC的控制层 -->
  <context:component-scan base-package="放置控制层的全限定包名"></context:component-scan>
  <!-- 配置视图解析器 -->
  <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
      <property name="prefix" value="/WEB-INF/view/"></property>
      <property name="suffix" value=".jsp"></property>
</bean>
  ```

  ​			4、在POJO类上加入@Controller注解，SpringMVC就会将此类作为控制层加载，让其处理请求响应。
  
  ```java
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.RequestMapping;
  @Controller
  public class TestController {
      @RequestMapping("hello")
      public String hello(){
          //处理请求结束之后跳转到“/WEB-INF/view/success.jsp”页面
          return "success";
      }
}
  ```

  ​			5、在控制层中，需要在方法上设置@RequestMapping(value="xxx")，SpringMVC就是通过此注释将请求路径与控制层的方法进行匹配，此时请求路径为localhost:8080/项目名/xxx。
  
  ​			6、处理请求的方法会返回一个字符串，即视图名称，最终会通过配置文件中配置的视图解析器实现页面跳转，方式：prefix + 视图名称 + suffix，此为最终跳转的页面路径。

## 二、@RequestMapping映射请求注解

### 1、作用

> 设置请求映射，把请求和控制层中的方法，设置映射关系当请求路径和@RequestMapping的value属性值一致时，则该注解所标注的方法即为处理请求的方法

### 2、常用属性

> - method：用来设置请求方式，只有客户端发送请求的方式和method的值一致时，才能处理请求
>
>   - 请求方式分类：
>
>     - GET	       查询
>     - POST         添加
>     - PUT           修改
>     - DELETE     删除
>
>   ```java
>    // 处理GET请求
>    @RequestMapping(value = "hello",method = RequestMethod.GET)
>    // 处理POST请求
>    @RequestMapping(value = "hello",method = RequestMethod.POST)
>   ```
>
> - params：用来设置客户端传到服务器的数据，支持表达式
>      - 例如：
>     - params = {"username"}--->表示请求路径上必须包含username参数
>        - params = {"username!=123"}--->表示请求路径上必须包含username参数，并且值不能为123
>        - params = {"username=admin"}--->表示请求路径上必须包含username参数，并且值为admin
> 
> ```java
> // 表示的请求路径中必须包含username参数
> @RequestMapping(value = "hello",params = {"username"})
>  // 表示的请求路径中必须包含username和password参数，且password字段的值不能为123
>    @RequestMapping(value = "hello",params = {"username","password!=123"})
> ```
>    
>    - headers：用来设置请求头信息，所发送请求的请求头信息一定要和headers属性中所设置的一致
>  
>```java
> // 当请求头的字段Accept-Language的值必须和headers中设置的值保持一致
>@RequestMapping(value = "hello",headers = {"Accept-Language=zh-CN,zh;q=0.9,en;q=0.8"})
>    ```

### 3、可放置的位置

> 可以加在类上面，也可以加在方法上面
>
> - 若类和方法上都加入了该注解，那么请求应该一层一层访问，先访问类，再访问方法

```java
// java
@RequestMapping(value = "/test")
public class TestController {
	@RequestMapping(value = "/hello")
    public String hello(){return:"success"}
}
// index.jsp
<a href="test/hello">测试springmvc</a>
```

### 4、支持Ant风格路径

> - *：表示任意字符
> - ?：表示任意一个目录
> - **：表示任意多层目录

```java
// 该路径表示：/该层目录下任意字符/该层目录下ant加上任意两个字符/可以包含多层目录/testAnt
@RequestMapping(value = "/*/ant??/**/testAnt")
```

### 5、支持占位符方式路径

- @PathVariable注解

  > {xxx}：路径占位符

```java
// 以前：localhost:8080/spring/testRest?id=1001&username=admin
// 现在：localhost:8080/spring/testRest/1001/admin
@RequestMapping("testRest/{id}/{username}")
public String testRest(@PathVariable("id") Integer id,@PathVariable("username") String username){
    System.our.print("id = " + id + ",username" + username);
    return "success";
}
```

## 三、REST

### 1、参考资料

- [理解REST架构风格](https://kb.cnblogs.com/page/186516/)
- [REST](http://www.infoq.com/cn/articles/rest-introduction)

### 2、REST是什么

> 1）REST：即Representational State Transfer。（资源）表现层状态转化，是目前最流行的一种互联网软件架构。
>
> - 资源（Resources）：网络上的一个实体，或者说是网络上的一个具体信息。
> - 表现层（Representation）：把资源具体呈现出来的形式，叫做它的表现层（Representation）
> - 状态转化（State Transfer）：每发出一个请求，就代表了客户端和服务器的一次交互过程。HTTP协议，是一个无状态协议，即所有的状态都保存在服务器端。
> - 具体说，就是HTTP协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE
>
> 2）URL风格：
>
> - /order/1	HTTP GET：得到id=1 的order
> - /order/1	HTTP DELETE：删除id=1 的order
> - /order	HTTP PUT：更新order
> - /order	HTTP POST：新增order
>

### 3、HiddenHttpMethodFilter介绍

> - 浏览器form表单只支持GET与POST请求，而DELETE、PUT等method并不支持。
>
>   Spring3.0添加了一个过滤器，可以将这些请求转换为标准的http方法。
>
> - org.springframework.web.filter.HiddenHttpMethodFilter会根据几个条件对POST请求做出转换
>
>   条件：① POST		② 参数：_method 

### 4、HiddenHttpMethodFilter过滤器源码分析

- 1）为什么请求隐含参数名称必须叫做“_method”

  ```java
  public static final String DEFAULT_METHOD_PARAM = "_method";
  private String methodParam = "_method";
  ```

- 2）HiddenHttpMethodFilter的处理过程

  ```java
  protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
  	HttpServletRequest requestToUse = request;
  	if ("POST".equals(request.getMethod()) && request.getAttribute("javax.servlet.error.exception") == null) {
  		String paramValue = request.getParameter(this.methodParam);
  		if (StringUtils.hasLength(paramValue)) {
  			String method = paramValue.toUpperCase(Locale.ENGLISH);
  			if (ALLOWED_METHODS.contains(method)) {
  				requestToUse = new HiddenHttpMethodFilter.HttpMethodRequestWrapper(request, method);
  			}
  		}
  	}
  	filterChain.doFilter((ServletRequest)requestToUse, response);
  }
  ```

  ```java
  private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {
  	private final String method;
  	public HttpMethodRequestWrapper(HttpServletRequest request, String method) {
  		super(request);
  		this.method = method;
  	}
  	public String getMethod() {
  		return this.method;
  	}
  }
  ```

### 5、代码实现

```xml
<filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <!--/*表示根路径下的所有请求-->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

```java
@RequestMapping(value = "get",method = RequestMethod.GET)
public String getRequest(){
    return "success";
}
@RequestMapping(value = "post",method = RequestMethod.POST)
public String postRequest(String _method){
    System.out.println("METHOD" = _method);
    return "success";
}
@RequestMapping(value = "put",method = RequestMethod.PUT)
public String putRequest(String _method){
    System.out.println("METHOD" = _method);
    return "success";
}
@RequestMapping(value = "delete",method = RequestMethod.DELETE)
public String deleteRequest(String _method){
    System.out.println("METHOD" = _method);
    return "success";
}
```

```html
<!--GET-->
<a href="hello">testGetRequest</a>
<!--POST-->
<form action="testRest" method="POST">
    <input type="submit" value="testPostRequest" />
</form>
<!--PUT-->
<form action="testRest" method="POST">
    <input type="hidden" name="_method" value="PUT" />
    <input type="submit" value="testPutRequest" />
</form>
<!--DELETE-->
<form action="testRest" method="POST">
    <input type="hidden" name="_method" value="DELETE" />
    <input type="submit" value="testDeleteRequest" />
</form>
```

## 四、处理请求数据

- 请求数据：请求参数、cookie信息、请求头信息...
- Java web：
  - HttpServletRequest
    - Request.getParameter(参数名);
    - Request.getParameterMap();
    - Request.getCookies();
    - Request.getHeader();

### 1、请求处理方法签名

> 1）Spring MVC通过分析处理方法的签名（方法名 + 参数列表），HTTP请求信息绑定到处理方法的相应形参中。
>
> 2）Spring MVC对控制器处理方法签名的限制很宽松，几乎可以按喜欢的任何方式对方法进行签名
>
> 3）必要时可以对方法及方法形参标注相应的注解（@PathVariable、@RequestParam、@RequestHeader等）
>
> 4）Spring MVC框架会将HTTP请求的信息绑定到相应的方法形参中，并根据方法的返回值类型做出相应的后续处理。

### 2、@RequestParam形参注解

> 在SpringMVC获取客户端传递数据方式：
>
> 在处理请求的方法中，加入相对应的形参，保证形参参数名和传递数据的参数名保持一致，就可以自动赋值

- value：当不满足赋值条件时，可以使用value属性，指定映射关系。

- required：设置形参是否必须被赋值，默认为true，必须赋值/存在，若设置为false，则不必需赋值，因此形参的值为null。

- defaultValue：若形参所获得的值为null，则设置一个默认值，用在分页和模糊查询中居多。

```jsp
<form action="param" method="post">
    username：<input type="text" name="username"><br>
    password：<input type="password" name="password">
    <input type="submit" value="submit">
</form>
```

```java
@RequestMapping(value = "/param")
public String doSome(@RequestParam(value = "username",required = false,defaultValue = 							"zhangxinrun") String username ,
                     @RequestParam(value = "password",required = true) String 								  password){
    System.out.println("username = " + username + ",password = " + password);
    return "success";
}
```

### 3、@RequestHeader请求头注解

> 作用：在处理请求的方法上，来获取请求头信息，用法和@RequestParam一样
>
> - 使用@RequestHeader绑定请求报头的属性值
> - 请求报头包含了若干个属性，服务器可根据此属性值获取客户端的信息

```java
@RequestMapping(value = "/getCookieValue")
public String getCookieValue(@RequestHeader(value = "Cookie") String Cookie){
    System.out.println("Cookie = " + Cookie);
    return "success";
}
```

### 4、@CookieValue注解

> - 使用@CookieValue绑定请求中的Cookie值
> - @CookieValue可让处理方法形参绑定某个Cookie值

```java
@RequestMapping(value = "/getJSESSIONID")
//public String getJSESSIONID(@CookieValue() String JSESSIONID){	或
public String getJSESSIONID(@CookieValue(value = "JSESSIONID") String JSESSIONID){
    System.out.println("JSESSIONID = " + JSESSIONID);
    return "success";
}
```

### 5、使用POJO作为参数

> - 使用POJO对象绑定请求参数值
> - Spring MVC会按照请求参数名和POJO属性名进行自动匹配，自动为该对象填充属性值，支持级联赋值，
>   - 如：dept.deptId、dept.address.tel等等

```java
// pojo
public class User {
    private String username;
    private String password;
	public User() {}
	public User(String username, String password) {
        this.username = username; this.password = password;
    }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
	@Override
    public String toString() {
        return "User{" + "username='" + username + '\'' + ", password='" + password + '\'' + '}';
    }
}
```

```jsp
<form action="user" method="post">
    username：<input type="text" name="username"><br>
    password：<input type="password" name="password">
    <input type="submit" value="submit">
</form>
```

```java
@RequestMapping(value = "user")
public String user(User user){
	System.out.println("username = " + user.getUsername() + ",password = " + user.getPassword());
    return "success";
}
```

### 6、使用Servlet原生API作为参数

> - MVC的Header方法可以接收哪些ServletAPI类型的参数
>   - HttpServletRequest / HttpServletResponse / HttpSession
>   - InputStream / OutputStream / Reader / Writer
>   - Locale
>   - java.security.Principal
> - 源码参考：AnnotationMethodHandlerAdapter

```java
@RequestMapping(value = "user")
public String user(HttpServletRequest request,HttpServletResponse response){
	String username = request.getParameter("username");
    String password = request.getParameter("password");
    System.out.println("username = " + username + ",password = " + password);
    return "success";
}
```

## 五、ModelAndView处理响应数据

### 1、SpringMVC输出模型数据

> ① ModelAndView：处理方法返回值类型为ModelAndView时，方法体可通过该对象添加模型数据。
>
> ② Map 或 Model：形参为org.springframework.ui.Model、org.springframework.ui.ModelMap、Java.util.Map时，处理方法返回时，Map中的数据会自动添加到模型中。

### 2、ModelAndView介绍

### 3、ModelAndView源码理解

### 4、三种方式实现作用域设值

```jsp
username = ${username}
```

> - 第一种方式

```java
@RequestMapping(value = "/modelAndView1")
public ModelAndView modelAndView1(){
    ModelAndView mv = new ModelAndView();
    mv.addObject("username","zhangxinrun");		//往request作用域中放值
    mv.setViewName("success");			//设置视图名称，实现页面跳转
    return mv;
}
```

> - 第二种方式

```java
@RequestMapping(value = "/modelAndView2")
public String modelAndView2(Map<String, Object> map){
    map.put("username","zhangxinrun");		//向作用域中放值
    return "success";		//返回视图名称
}
```

> - 第三种方式

```java
@RequestMapping(value = "/modelAndView3")
public String modelAndView3(Model model){
    // Map<String,Object> map = new HashMap<>();
    // map.put("username","zhangxinrun");
    // model.addAllAttributes(map);
    model.addAttribute("username","zhangxinrun");		//向作用域中放值
    return "success";		//返回视图名称
}
```

### 5、总结

> 不管使用以上的哪种方式，最终都会把model数据和view数据封装到一个ModelAndView中

## 六、视图解析

### 1、Spring MVC如何解析视图

> ① 不论控制器返回一个String，ModelAndView，view都会转换为ModelAndView对象，由视图解析器解析视图，然后，进行页面的跳转
>
> ② 视图解析器源码分析：两个重要接口

### 2、重定向

> ① 一般情况下，控制器方法返回字符串的值会被当做逻辑视图名处理。
>
> ② 如果返回的字符串中带 forward: 或 redirect: 前缀时，SpringMVC会对他们进行特殊处理；
>
> ​	将 forward: 和 redirect: 当成指示符，其后的字符串作为URL来处理
>
> ③ redirect:success.jsp 会完成一个到 success.jsp 的重定向的操作
>
> ④ forward:success.jsp 会完成一个到 success.jsp 的转发操作

### 3、Spring MVC指定配置文件位置和名称

```xml
<servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--设置springMVC配置文件的位置以及名称-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <!--指定类路径下有一个文件applicationContext.xml作为控制器的配置文件-->
        <param-value>classpath:applicationContext.xml</param-value>
    </init-param>
    <!--项目启动时加载-->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

### 4、Spring中的编码过滤器

> - org.springframework.web.filter.CharacterEncodingFilter
>   - 必须写在所有的过滤器的第一个

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceRequestEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
    <init-param>
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 七、静态资源处理

```xml
<!--
	配置Tomcat中默认的servlet（org.apache.catalina.servlets.DefaultServlet）
    注意：当DefaultServlet所设置的<url-pattern>的值和开发人员所配置的servlet的<url-pattern>相同，以开发人员所配置的servlet优先
    作用：当客户端发送请求，由于DefaultServlet所设置的<url-pattern>的值和开发人员所配置的DispatcherServlet的<url-pattern>都是/
    因此先通过DispatcherServlet处理请求，找该请求是否有相对应的处理器，有则处理，无则交给DefaultServlet处理
-->
<mvc:default-servlet-handler></mvc:default-servlet-handler>
<!--或-->
<mvc:default-servlet-handler />
```

```xml
<!--注解驱动-->
<mvc:annotation-driven></mvc:annotation-driven>
```

## 八、处理JSON

> json知识：
>
> 1. json有两种格式：
>    - json对象:{key:value, key:value...}
>    - json数组:[value1, value2...]
> 2. json对象解析方式：对象.key；json数组解析方式：for循环遍历
> 3. Java对象转换为json：
>    - bean和map--->json对象
>    - List--->json数组

### 1、返回JSON

> - 加入jar包：
>
>   - jackson-annotations-2.1.5.jar
>   - jackson-core-2.1.5.jar
>   - jackson-databind-2.1.5.jar
>
> - 在Spring MVC的配置文件中开启MVC注解驱动\<mvc:annotation-driven\\\>
>
>   ```xml
>   <mvc:annotation-driven />
>   ```
>
> - 在处理ajax请求的方法上加上注解@ResponseBody
>
>   ```java
>   @RequestMapping("/test")
>   @ResponseBody
>   public String ret(){
>       return "returnJsonObject";
>   }
>   ```
>
> - 将要转换为json且响应到客户端的数据，直接作为该方法的返回值返回

### 2、@ResponseBody





  文件下载

```jsp
<a href="downloadFile">文件下载</a>
```

```java
@RequestMapping(value = "downloadFile")
```

文件上传

```jsp
<form action="upLoadFile" method="post" >
    头像：<input type="file" name="uploadFile" /><br />
    描述：<input type="text" name="desc" /><br />
    <input type="submit" value="上传文件" />
</form>
```

```java
@RequestMapping()
public String upLoadFile(){
    
}
```

```xml
<!--springMVC.xml-->
<!--处理文件，将客户端上传的File文件，处理为multipartResolver，-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
	<!--设置文件解析的编码，注意：一定要和页面的pageEncoding保持一致-->
    <property name="defaultEncoding" value="utf-8"></property>
	<!--设置最大上传大小-->
    <property name="maxUploadSize" value="888888888888"></property>
</bean>
```





  











  

  

  

  

  

  

  

  



































