# Shiro

## 一、权限的管理

### 1、什么是权限管理

> ​		基本上涉及到用户参与的系统都要进行权限管理，权限管理属于系统安全的范畴，权限管理实现==对用户访问系统的控制==，按照安全规则或者安全策略控制用户可以访问而且只能访问自己被授权的资源。
>
> ​		权限管理包括==身份认证==和==授权==两部分，简称==认证授权==，对于需要访问控制的资源用户首先经过身份认证，认证通过后用户具有该资源的访问权限方可访问。

### 2、什么是身份认证

> ​		==身份认证==，就是判断一个用户是否为合法用户的处理过程，最常用的简单身份认证方式是系统通过核对用户输入的用户名和口令，看其是否与系统中存储的该用户的用户名和口令一致，来判断用户身份是否正确。对于采用指纹等系统，则出示指纹；对于硬件key等刷卡系统，则需要刷卡。

### 3、什么是授权

> ​		==授权==，即访问控制，控制谁能访问哪些资源。主体进行身份认证后需要分配权限方可访问系统的资源，对于某些资源没有权限是无法访问的。

## 二、什么是shiro

> ​	 **Apache Shiro™** is a powerful and easy-to-use Java security framework that performs authentication, authorization, cryptography, and session management. With Shiro’s easy-to-understand API, you can quickly and easily secure any application – from the smallest mobile applications to the largest web and enterprise applications. 

 	==**Apache Shiro™**是一个功能强大且易于使用的Java安全框架，用于执行身份验证，授权，加密和会话管理。使用Shiro易于理解的API，您可以快速轻松地保护任何应用程序，从最小的移动应用程序到最大的Web和企业应用程序。== 

![ShiroArchitecture](images/ShiroArchitecture.png)

## 三、shiro的核心架构

- **Subject** ([`org.apache.shiro.subject.Subject`](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/subject/Subject.html))
  A security-specific ‘view’ of the entity (user, 3rd-party service, cron job, etc) currently interacting with the software.
- **SecurityManager** ([org.apache.shiro.mgt.SecurityManager](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/mgt/SecurityManager.html))
  As mentioned above, the `SecurityManager` is the heart of Shiro’s architecture. It is mostly an ‘umbrella’ object that coordinates its managed components to ensure they work smoothly together. It also manages Shiro’s view of every application user, so it knows how to perform security operations per user.
- **Authenticator** ([org.apache.shiro.authc.Authenticator](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/authc/Authenticator.html))
  The `Authenticator` is the component that is responsible for executing and reacting to authentication (log-in) attempts by users. When a user tries to log-in, that logic is executed by the `Authenticator`. The `Authenticator` knows how to coordinate with one or more `Realms` that store relevant user/account information. The data obtained from these `Realms` is used to verify the user’s identity to guarantee the user really is who they say they are.
  - **Authentication Strategy** ([org.apache.shiro.authc.pam.AuthenticationStrategy](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/authc/pam/AuthenticationStrategy.html))
    If more than one `Realm` is configured, the `AuthenticationStrategy` will coordinate the Realms to determine the conditions under which an authentication attempt succeeds or fails (for example, if one realm succeeds but others fail, is the attempt successful? Must all realms succeed? Only the first?).
- **Authorizer** ([org.apache.shiro.authz.Authorizer](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/authz/Authorizer.html))
  The `Authorizer` is the component responsible determining users’ access control in the application. It is the mechanism that ultimately says if a user is allowed to do something or not. Like the `Authenticator`, the `Authorizer` also knows how to coordinate with multiple back-end data sources to access role and permission information. The `Authorizer` uses this information to determine exactly if a user is allowed to perform a given action.
- **SessionManager** ([org.apache.shiro.session.mgt.SessionManager](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/session/mgt/SessionManager.html))
  The `SessionManager` knows how to create and manage user `Session` lifecycles to provide a robust Session experience for users in all environments. This is a unique feature in the world of security frameworks - Shiro has the ability to natively manage user Sessions in any environment, even if there is no Web/Servlet or EJB container available. By default, Shiro will use an existing session mechanism if available, (e.g. Servlet Container), but if there isn’t one, such as in a standalone application or non-web environment, it will use its built-in enterprise session management to offer the same programming experience. The `SessionDAO` exists to allow any datasource to be used to persist sessions.
  - **SessionDAO** ([org.apache.shiro.session.mgt.eis.SessionDAO](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/session/mgt/eis/SessionDAO.html))
    The `SessionDAO` performs `Session` persistence (CRUD) operations on behalf of the `SessionManager`. This allows any data store to be plugged in to the Session Management infrastructure.
- **CacheManager** ([org.apache.shiro.cache.CacheManager](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/cache/CacheManager.html))
  The `CacheManager` creates and manages `Cache` instance lifecycles used by other Shiro components. Because Shiro can access many back-end data sources for authentication, authorization and session management, caching has always been a first-class architectural feature in the framework to improve performance while using these data sources. Any of the modern open-source and/or enterprise caching products can be plugged in to Shiro to provide a fast and efficient user-experience.
- **Cryptography** ([org.apache.shiro.crypto.*](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/crypto/package-summary.html))
  Cryptography is a natural addition to an enterprise security framework. Shiro’s `crypto` package contains easy-to-use and understand representations of crytographic Ciphers, Hashes (aka digests) and different codec implementations. All of the classes in this package are carefully designed to be very easy to use and easy to understand. Anyone who has used Java’s native cryptography support knows it can be a challenging animal to tame. Shiro’s crypto APIs simplify the complicated Java mechanisms and make cryptography easy to use for normal mortal human beings.
- **Realms** ([org.apache.shiro.realm.Realm](http://shiro.apache.org/static/current/apidocs/org/apache/shiro/realm/Realm.html))
  As mentioned above, Realms act as the ‘bridge’ or ‘connector’ between Shiro and your application’s security data. When it comes time to actually interact with security-related data like user accounts to perform authentication (login) and authorization (access control), Shiro looks up many of these things from one or more Realms configured for an application. You can configure as many `Realms` as you need (usually one per data source) and Shiro will coordinate with them as necessary for both authentication and authorization.

## 四、shiro中的认证

### 1、认证

> ​		身份认证，就是判断一个用户是否为合法的处理过程。最常用的简单身份认证方式是系统通过核对用户输入的用户名和口令，看其是否与系统中存储的该用户的用户名和口令一致，来判断用户身份是否正确。

### 2、shiro中认证的关键对象

- Subject：主体

  > ==访问系统的用户==，主体可以是用户、程序等，进行认证的都称为主体

- Principal：身份信息

  > 是主体（Subject）进行==身份认证的标识==，标识必须具有唯一性，如用户名、手机号、邮箱地址等，一个主体可以有多个身份，但是必须有一个主身份（Primary Principal）

- Credential：凭证信息

  > 是==只有主体自己知道的安全信息==，如密码、证书等

### 3、认证流程

![1616430780038](images/1616430780038.png)

### 4、认证的开发

- 创建maven项目（普通Java工程）

  ```xml
  <!--引入shiro依赖-->
  <dependency>    
      <groupId>org.apache.shiro</groupId>    
      <artifactId>shiro-core</artifactId>    
      <version>1.5.3</version>
  </dependency>
  ```

- 引入shiro配置文件并加入如下配置

  ```ini
  [users]
  xiaozhang=123
  ```

- 开发认证代码

  ```java
  import org.apache.shiro.SecurityUtils;
  import org.apache.shiro.authc.IncorrectCredentialsException;
  import org.apache.shiro.authc.UnknownAccountException;
  import org.apache.shiro.authc.UsernamePasswordToken;
  import org.apache.shiro.mgt.DefaultSecurityManager;
  import org.apache.shiro.mgt.SecurityManager;
  import org.apache.shiro.realm.text.IniRealm;
  import org.apache.shiro.subject.Subject;
  
  public class MyAuthenticator {
      public static void main(String[] args) {
          //1、创建安全管理器对象
          SecurityManager securityManager = new DefaultSecurityManager();
          //2、给安全管理器设置realm
          ((DefaultSecurityManager) securityManager).setRealm(new IniRealm("classpath:shiro.ini"));
          //3、SecurityUtils 给全局的安全工具类设置安全管理器
          SecurityUtils.setSecurityManager(securityManager);
          //4、关键对象 subject 主体
          Subject subject = SecurityUtils.getSubject();
          //5、创建令牌
          UsernamePasswordToken token = new UsernamePasswordToken("xiaozhang","123");
          try{
              System.out.println("认证前，认证状态：" + subject.isAuthenticated());
              subject.login(token);    //用户认证
              System.out.println("认证后，认证状态：" + subject.isAuthenticated());
          }catch (UnknownAccountException e){
              e.printStackTrace();
              System.out.println("认证失败：用户名不存在...");  // UnknownAccountException
          }catch (IncorrectCredentialsException e){
              e.printStackTrace();
              System.out.println("认证失败：密码错误...");    // IncorrectCredentialsException
          }
      }
  }
  
  ```

## 五、自定义Realm

### 1、shiro提供的Realm

![1616506948875](images/1616506948875.png)

### 2、根据认证源码认证使用的是SimpleAccountReaml

![1616507239685](images/1616507239685.png)

> SimpleAccountRealm的部分源码中有两个方法：==一个是认证，一个是授权；==

### 3、自定义realm开发

- 使用自定义的realm

```java
import com.zhang.realm.CustomerRealm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

/**
 * 使用自定义Realm
 */
public class TestCustomerAuthorizingRealm {
    public static void main(String[] args) {
        //创建securityManager
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //设置自定义realm
        defaultSecurityManager.setRealm(new CustomerRealm());
        //设置安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //通过安全工具获取subject
        Subject subject = SecurityUtils.getSubject();
        //创建token
        UsernamePasswordToken token = new UsernamePasswordToken("xiaozhang","123");
        try{
            subject.login(token);
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }
    }
}

```

- 自定义realm实现

```java
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;

/**
 * 自定义realm实现   将认证/授权数据的来源转为数据库的实现
 */
public class CustomerRealm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        // 在token中获取用户名
        String principal = (String) token.getPrincipal();
        System.out.println("用户名 = " + principal);
        //根据身份信息使用jdbc  mybatis查询相关数据库
        if("xiaozhang".equals(principal)){
            // 参数1：返回数据库中的用户名   
            // 参数2：返回数据库中的正确密码     
            // 参数3：提供当前realm的名字    this.getName();
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,"123",this.getName());
            return simpleAuthenticationInfo;
        }
        return null;
    }
}
```

## 六、MD5算法

### 1、MD5算法的基本使用

- 普通md5算法

```java
import org.apache.shiro.crypto.hash.Md5Hash;

public class TestShiroMD5 {
    public static void main(String[] args) {
        //创建一个md5算法
        //普通md5算法
        Md5Hash md5Hash = new Md5Hash("123");
        String s = md5Hash.toHex();//转换为16进制
        System.out.println("md5算法 = " + s);   //202cb962ac59075b964b07152d234b70
    }
}
```

- 使用MD5 + salt处理

```java
import org.apache.shiro.crypto.hash.Md5Hash;

public class TestShiroMD5 {
    public static void main(String[] args) {
        //使用MD5 + salt处理
        Md5Hash md5Hash1 = new Md5Hash("123","X0*7pd");
        System.out.println("使用MD5 + salt处理 = " + md5Hash1.toHex());     //1e680a7d41bd4e51951d9a049b09409f
    }
}
```

- 使用MD5 + salt + hash散列

```java
import org.apache.shiro.crypto.hash.Md5Hash;

public class TestShiroMD5 {
    public static void main(String[] args) {
        //使用MD5 + salt + hash散列
        Md5Hash md5Hash2 = new Md5Hash("123","X0*7pd",1024);
        System.out.println("使用MD5 + salt + hash散列 = " + md5Hash2.toHex());  //d093bbd2eeba922b875c4dd2dfc695dc
    }
}
```

### 2、MD5算法认证方式实现

- 普通md5算法

```java
import com.zhang.realm.CustomerMD5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class TestCustomerMD5RealmAuthenicator {
    public static void main(String[] args) {
        //创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //注入realm
        CustomerMD5Realm realm = new CustomerMD5Realm();
        //设置realm使用的hash凭证匹配器
        defaultSecurityManager.setRealm(realm);
        //将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //通过安全工具获取subject
        Subject subject = SecurityUtils.getSubject();
        //认证
        UsernamePasswordToken token = new UsernamePasswordToken("xiaozhang","123");
        try {
            subject.login(token);
            System.out.println("登录成功");
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }
    }
}
```

> 自定义的Realm类

```java
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

public class CustomerMD5Realm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        String principal = (String) token.getPrincipal();
        if("xiaozhang".equals(principal)){
            // 使用md5算法
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,"202cb962ac59075b964b07152d234b70",this.getName
            return simpleAuthenticationInfo;
        }
        return null;
    }
}
```

- 使用MD5 + salt处理

```java
import com.zhang.realm.CustomerMD5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class TestCustomerMD5RealmAuthenicator {
    public static void main(String[] args) {
        //创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //注入realm
        CustomerMD5Realm realm = new CustomerMD5Realm();
        //设置realm使用的hash凭证匹配器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        // 指定哪种算法
        credentialsMatcher.setHashAlgorithmName("md5");
        realm.setCredentialsMatcher(credentialsMatcher);
        defaultSecurityManager.setRealm(realm);
        //将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //通过安全工具获取subject
        Subject subject = SecurityUtils.getSubject();
        //认证
        UsernamePasswordToken token = new UsernamePasswordToken("xiaozhang","123");
        try {
            subject.login(token);
            System.out.println("登录成功");
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }
    }
}
```

> 自定义的Realm类

```java
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

public class CustomerMD5Realm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        String principal = (String) token.getPrincipal();
        if("xiaozhang".equals(principal)){
            // 使用md5算法 + salt
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,"1e680a7d41bd4e51951d9a049b09409f",
                     ByteSource.Util.bytes("X0*7pd"),this.getName());
            return simpleAuthenticationInfo;
        }
        return null;
    }
}
```

- 使用MD5 + salt + hash散列

```java
import com.zhang.realm.CustomerMD5Realm;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.subject.Subject;

public class TestCustomerMD5RealmAuthenicator {
    public static void main(String[] args) {
        //创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //注入realm
        CustomerMD5Realm realm = new CustomerMD5Realm();
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        // 指定哪种算法
        credentialsMatcher.setHashAlgorithmName("md5");
        // 设置散列次数
        credentialsMatcher.setHashIterations(1024);
        //设置realm使用的hash凭证匹配器
        realm.setCredentialsMatcher(credentialsMatcher);
        defaultSecurityManager.setRealm(realm);
        //将安全管理器注入安全工具
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //通过安全工具获取subject
        Subject subject = SecurityUtils.getSubject();

        //认证
        UsernamePasswordToken token = new UsernamePasswordToken("xiaozhang","123");
        try {
            subject.login(token);
            System.out.println("登录成功");
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("用户名错误");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误");
        }
    }
}
```

> 自定义的Realm类

```java
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authc.SimpleAuthenticationInfo;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.util.ByteSource;

public class CustomerMD5Realm extends AuthorizingRealm {
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        String principal = (String) token.getPrincipal();
        if("xiaozhang".equals(principal)){
            // 使用md5算法 + salt + hash散列
            SimpleAuthenticationInfo simpleAuthenticationInfo = new SimpleAuthenticationInfo(principal,"d093bbd2eeba922b875c4dd2dfc695dc",
                    ByteSource.Util.bytes("X0*7pd"),this.getName());
            return simpleAuthenticationInfo;
        }
        return null;
    }
}
```

## 七、shiro的授权

### 1、授权

> ​		授权，即==访问控制==，控制谁能访问哪些资源。主体进行身份认证后需要分配权限方可访问系统的资源，对于某些资源没有权限是没法访问的。

### 2、关键对象

> ==授权可简单理解为who对what（which）进行how操作==

- ==Who，即主体（Subject）==，主体需要访问系统中的资源
- ==What，即资源（Resource）==，如系统菜单、页面、按钮、类方法、系统商品信息等。资源包括==资源类型==和==资源实例==
- ==How，权限/许可（Permission）==，规定了主体对资源的操作许可，权限离开资源没有意义，如用户查询权限、用户添加权限、某个类方法的调用权限，通过权限可知主体对哪些资源都有哪些操作许可。

### 3、授权的流程

### 4、授权方式

- 基于角色的访问控制

  - RBAC基于角色的访问控制（Role-Based Access Control）是以角色为中心进行范围控制

    ```java
    if(subject.hasRole("admin")){
        //操作什么资源
    }
    ```

- 基于资源的访问控制

  - RBAC基于资源的访问控制（Resource-Based Access Control）是以资源为中心进行访问控制

    ```java
    if(subject.isPermission("user:*:create")){
        //对所有用户具有创建
    }
    ```

### 5、权限字符串

> ​		权限字符串规则是：**==资源修饰符：操作：资源实例==**标识符，意思是对哪个资源的哪个实例具有什么操作，“：”是资源/操作/实例的分隔符，权限字符串也可以使用“*”通配符

- 例子：
  - 用户创建权限：`user:create`，或`user:create:*`
  - 用户修改实例001的权限：`user:update:001`
  - 用户实例001的所有权限：`user:*:001`

### 6、shiro中权限编程实现方式

- 编程式
- 注解式
- 标签式

















