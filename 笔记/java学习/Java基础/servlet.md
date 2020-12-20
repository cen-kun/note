# Servlet

### Servlet定义及使用

* 什么是 Servlet？

Servlet 是 Java Web 开发的基⽯，与平台⽆关的服务器组件，它是运⾏在 Servlet 容器/Web 应⽤服务
器/Tomcat，负责与客户端进⾏通信。

* Servlet 的功能
    1.  创建并返回基于客户请求的动态 HTML ⻚⾯。
    2.  与数据库进⾏通信。

* 如何使⽤ Servlet？

Servlet 本身是⼀组接⼝，⾃定义⼀个类，并且实现 Servlet 接⼝，这个类就具备了接受客户端请求以及
做出响应的功能。

```java
package com.southwind.servlet;import javax.servlet.*;
import java.io.IOException;
public class MyServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
    }
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }
    @Override
    public void service(ServletRequest servletRequest, ServletResponse 
servletResponse) throws ServletException, IOException {
        String id =  servletRequest.getParameter("id");
        System.out.println("我是Servlet，我已经接收到了客户端发来的请求，参数是"+id);
        servletResponse.setContentType("text/html;charset=UTF-8");
        servletResponse.getWriter().write("客户端你好，我已接收到你的请求");
    }
    @Override
    public String getServletInfo() {
        return null;
    }
    @Override
    public void destroy() {
    }
}
```

浏览器不能直接访问 Servlet ⽂件，只能通过映射的⽅式来间接访问 Servlet，映射需要开发者⼿动配
置，有两种配置⽅式。

* 基于 XML ⽂件的配置⽅式。

```xml
<servlet>
  <servlet-name>hello</servlet-name>
  <servlet-class>com.southwind.servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>hello</servlet-name>
  <url-pattern>/demo2</url-pattern>
</servlet-mapping>
```

* 基于注解的⽅式。

```java
@WebServlet("/demo2")
public class HelloServlet implements Servlet {  
}
```

上述两种配置⽅式结果完全⼀致，将 demo2 与 HelloServlet 进⾏映射，即在浏览器地址栏中直接访问
demo 就可以映射到 HelloServlet。

### Servlet 的⽣命周期

1. 当浏览器访问 Servlet 的时候，Tomcat 会查询当前 Servlet 的实例化对象是否存在，如果不存在，
    则通过反射机制动态创建对象，如果存在，直接执⾏第 3 步。
2. 调⽤ init ⽅法完成初始化操作。
3. 调⽤ service ⽅法完成业务逻辑操作。
4. 关闭 Tomcat 时，会调⽤ destory ⽅法，释放当前对象所占⽤的资源。

> Servlet 的⽣命周期⽅法：⽆参构造函数、init、service、destory

* ⽆参构造函数只调⽤⼀次，创建对象。
* init 只调⽤⼀次，初始化对象。
* service 调⽤ N 次，执⾏业务⽅法。
* destory 只调⽤⼀次，卸载对象。

### ServletConﬁg

该接⼝是⽤来描述 Servlet 的基本信息的。

* `getServletName()` 返回 Servlet 的名称，全类名(带着包名的类名)
* `getInitParameter(String key)` 获取 init 参数的值（web.xml）
* `getInitParameterNames()` 返回所有的 initParamter 的 name 值，⼀般⽤作遍历初始化参数
* `getServletContext()` 返回 ServletContext 对象，它是 Servlet 的上下⽂，整个 Servlet 的管理者

>  ServletConﬁg 和 ServletContext 的区别：

* ServletConﬁg 作⽤于某个 Servlet 实例，每个 Servlet 都有对应的 ServletConﬁg，ServletContext 作⽤
    于整个 Web 应⽤，⼀个 Web 应⽤对应⼀个 ServletContext，多个 Servlet 实例对应⼀个ServletContext。
* ⼀个是局部对象，⼀个是全局对象。

### Servlet 的层次结构  

Servlet ---》GenericServlet ---〉HttpServlet
HTTP 请求有很多种类型，常⽤的有四种：

* GET 读取
* POST 保存
* PUT 修改
* DELETE 删除

1. `GenericServlet` 实现 `Servlet` 接⼝，同时为它的⼦类屏蔽了不常⽤的⽅法，⼦类只需要重写 service ⽅法即可。
2. `HttpServlet` 继承 GenericServlet，根据请求类型进⾏分发处理，GET 进⼊ doGET ⽅法，POST 进⼊doPOST ⽅法。
3. 开发者⾃定义的 Servlet 类只需要继承 `HttpServlet` 即可，重新 doGET 和 doPOST。