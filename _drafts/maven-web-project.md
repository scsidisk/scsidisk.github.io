使用 maven 创建 web 项目
=======================


### 创建项目

    $ mvn archetype:generate -DarchetypeArtifactId=maven-archetype-webapp
    ...
    [INFO] Generating project in Interactive mode
    Define value for property 'groupId': : com.example
    Define value for property 'artifactId': : helloworld

将在 helloworld 文件夹中创建项目。

### 配置项目使用 Jetty Runner

    <build>
      ...
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.3</version>
          <executions>
            <execution>
              <phase>package</phase>
              <goals><goal>copy</goal></goals>
              <configuration>
                <artifactItems>
                  <artifactItem>
                    <groupId>org.eclipse.jetty</groupId>
                    <artifactId>jetty-runner</artifactId>
                    <version>9.3.3.v20150827</version>
                    <destFileName>jetty-runner.jar</destFileName>
                  </artifactItem>
                </artifactItems>
              </configuration>
            </execution>
           </executions>
        </plugin>
      </plugins>
    </build>

### 运行项目

打包项目:

    $ mvn package

运行项目:

    $ java -jar target/dependency/jetty-runner.jar target/*.war

使用使用浏览器访问 http://localhost:8080.

### 添加登录

添加 src/main/java/UserServlet.java

    package com.example.web4.action;

    import java.io.IOException;
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    public class UserServlet extends HttpServlet {
        private static final long serialVersionUID = 1L;

        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            doPost(request , response);
        }

        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
            request.setCharacterEncoding("UTF-8");
            response.setContentType("text/html;charset=utf-8");

            String action = request.getParameter("action");
            if("login_input".equals(action)) {
                request.getRequestDispatcher("login.jsp").forward(request , response);
            } else if("login".equals(action)) {
                String name = request.getParameter("name");
                String password = request.getParameter("password");

                System.out.println("name->" + name + ",password->" + password);
            }
        }

    }

修改web.xml

    <!DOCTYPE web-app PUBLIC
     "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
     "http://java.sun.com/dtd/web-app_2_3.dtd" >

    <web-app>
      <display-name>Archetype Created Web Application</display-name>
      <servlet>
        <servlet-name>UserServlet</servlet-name>
        <servlet-class>com.example.web4.action.UserServlet</servlet-class>
      </servlet>
      <servlet-mapping>
        <servlet-name>UserServlet</servlet-name>
        <url-pattern>/user</url-pattern>
      </servlet-mapping>
    </web-app>

修改index.jsp

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
            <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
            <title>Hello Maven</title>
        </head>

        <body>
            <p>大家好！</p>
            <a href="user?action=login_input">去登录</a>
        </body>
    </html>

新建 login.jsp

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>登录界面</title>
        </head>

        <body>
            <form action="user?action=login" method="post">
                Name:<input type="text" name="name" />
                Password:<input type="password" name="password" />

                <input type="submit" value="登录" />
            </form>
        </body>
    </html>

添加依赖

    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>

打包并运行

    $ mvn package
    $ java -jar target/dependency/jetty-runner.jar target/*.war

使用使用浏览器访问 http://localhost:8080

输入用户名和密码后提交，命令行会输出

    INFO:oejs.Server:main: Started @867ms
    name->111,password->222