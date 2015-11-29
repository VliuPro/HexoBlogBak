title: HttpSession会话管理
date: 2015-10-10 19:44:43
categories: 
- Java
tags: 
- Java
- Servlet
description: 
---
> 会话管理有几种实现方式：Hidden Field（隐藏域） 、 Cookie 、 URL Rewriting（URL重写） 、 HttpSession

#### &#160; &#160; &#160; &#160;无论是哪种方式，都必须自行处理对浏览器的响应，决定哪些信息必须送至浏览器，以便在之后的请求一并发送相关信息，供Web应用程序辨识请求之间的关联

+  使用 `HttpServletRequest` 的 `getSession()` 方法取得对象。
    　　在运行 getSession() 方法时，Web容器会创建HttpSession对象，而每个HttpSession对象都有个特殊的ID 称为 `Session ID` ， 通过执行HttpSession的 getId() 方法来取得。Session ID默认使用Cookie存放在浏览器中，在Tomcat中，Cookie的名称时JSESSIONID，数值则是getId()所取得的Session ID。
>   
    ```Java
        HttpSession session = request.getSession();
        
        //传入true表示若尚未存在HttpSession实例就创建一个新的对象；若是传入false，则表示若尚未存在HttpSession实例就直接返回null。
        HttpSession session = request.getSession(true/false)
    ```
+  HttpSession上的常用方法：setAttribute() 和 getAttribute() ，和HttpServletRequest中的类似，可以在对象中设置和获取属性（这是可以存放属性对象的第二个地方，第三个是在ServletContext中）
     　　使用  setAttribute()  可以在会话期间保存这些信息，使用 getAttribute() 可以取出这些保存的信息。

+  默认在关闭浏览器之前，取得HttpSession都是相同的实例。如果想在这次会话期间直接让目前的HttpSession失效，可以执行HttpSession的 invalidate() 方法。（比如实现注销机制）
    ``session.invalidate()``
+  设定HttpSession自动失效的时间：（浏览器没有请求应用程序）
    * setMaxInactiveInterval()的时间单位：‘秒’
    ``session.setMaxInactiveInterval()``
    * 在web.xml中设定默认的失效时间（单位：‘分钟’）
>   
    ```XML
    <web-app ...>
        //省略
        
        <session-config>
            <session-timeout>30</session-timeout>
        </session-config>
    </web-app>
    ```
+  在 Servlet 3.0 中新增了 ``SessionCookieConfig`` 接口，可以通过 ServletContext 的``getSessionCookieConfig()`` 来取得实现该接口的对象。通过实现该接口的对象可以设定SessionOD的Cookie的相关信息：
    1. 通过setName()将默认的Session ID修改为其他的名称，
    2. 通过setAge()设定存储Session ID的Cookie存活期限，单位：‘秒’
>   
    ```XML
    //设定 SessionCookieConfig 必须在ServletContext 初始化之前。因此需要修改信息必须在web.xml中设定:
    
    <web-app ...>
        //省略
        <session-config>
            <session-timeout>30</session-timeout>
            <cookie-config>
                <name>sid-caterpillar</name>
                <http-only>true</http-only>
            </cookie-config>
        </session-config>
    </web-app>
    
    //另一个方法时实现 ServletContextListener，容器在初始化ServletContext时会调用ServletContextListener的contextInitialized()方法，可以在其中取得ServletContext进行SessionCookieConfig设定。
    ```
+  如果用户禁用了Cookie，可以搭配URL重写来使用HttpSession来进行会话管理
    　　使用URL重写来发送Session ID：使用 ``HttpServletResponse``的``encodeURL()``协助产生所需要的URL重写。
    　　容器尝试取得HttpSession实例时，若能从HTTP请求中取得带有Session ID的Cookie，encodeURL()会将传入的URL原封不动的输出，若无法取得，encodeURL()会自动产生带有Session ID的URL重写。
    ```Java
     request.getSession();
     
     out.print(response.encodeURL("index.jsp"));
    ```

    ---
　　总而言之：当容器尝试取得HttpSession对象时，无法从Cookie中取得Session ID。使用encodeURL()就会产生有Session ID的URL，以便于再次发送Session ID。另一个HttpServletResponse上的``encodeRedirectUrl()``方法择可以在要求浏览器重定向时在URL上显示Session ID
