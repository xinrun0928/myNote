# Spring

## 一、Spring概述





































































## 六、Spring 与 Web

> 在 Web 项目中使用 Spring 框架，首先**要解决在 web 层（这里指 Servlet）中获取到 Spring**
> **容器的问题**。只要在 web 层获取到了 Spring 容器，便可从容器中获取到 Service 对象

### 1、Web 项目使用 Spring 的步骤

#### 1）新建一个Maven Project工程

> 项目类型：**maven-archetype-webapp**

#### 2）代码编写步骤

- 创建Service 层、Dao 层

- pom.xml

  ```xml
  <dependencies>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.11</version>
          <scope>test</scope>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.2.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-tx</artifactId>
          <version>5.2.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-jdbc</artifactId>
          <version>5.2.5.RELEASE</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.1</version>
      </dependency>
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis-spring</artifactId>
          <version>1.3.1</version>
      </dependency>
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>5.1.9</version>
      </dependency>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.12</version>
      </dependency>
      <!-- servlet依赖 -->
      <dependency>
          <groupId>javax.servlet</groupId>
          <artifactId>javax.servlet-api</artifactId>
          <version>3.1.0</version>
          <scope>provided</scope>
      </dependency>
      <!-- jsp依赖 -->
      <dependency>
          <groupId>javax.servlet.jsp</groupId>
          <artifactId>jsp-api</artifactId>
          <version>2.2.1-b03</version>
          <scope>provided</scope>
      </dependency>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-web</artifactId>
          <version>5.2.5.RELEASE</version>
      </dependency>
  </dependencies>
  
  <build>
      <resources>
          <resource>
              <directory>src/main/java</directory><!--所在的目录-->
              <includes><!--包括目录下的.properties,.xml 文件都会扫描到-->
                  <include>**/*.properties</include>
                  <include>**/*.xml</include>
              </includes>
              <filtering>false</filtering>
          </resource>
      </resources>
      <plugins>
          <plugin>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.1</version>
              <configuration>
                  <source>1.8</source>
                  <target>1.8</target>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

- 配置文件 applicationContext.xml 及 jdbc.properties，mybatis.xml

- 定义index页面
- 