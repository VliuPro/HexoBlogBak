title: Servlet应用程序事件和监听器
date: 2015-10-14 11:05:31
tags: Java Web

---

+  ServletContext 事件、监听器
    　　与``ServletContext`` 相关的监听器有两种：``ServletContextListener`` 和 ``ServletContextAttributeListener``
    +   ServletContextListener —— "生命周期监听器"
        在Web应用程序初始化后或即将结束销毁前，会调用ServletContextListener 实现类相对应的``contextInitialized()`` 或 ``contextDestroyed()``。
        ```Java
            import java.util.EvebrListener;
            
            public interface ServletContextListener extends EventListener{
                public void contextInitialized(ServletContextEvent sce);
                public void contextDestroyed(ServletContextEvent sce);
            }
        ```

        　　ServletContextListener 可以直接用 ``@WebListener`` 标注，并且必须实现 ``ServletContextListener`` 接口。当Web容器调用contextInitialized()或contextDestroyed()时，会传入ServletContextEvent，其封装了ServletContext，可以通过它的getServletContext()方法取得ServletContext，通过ServletContext的getInitParameter()方法读取初始参数。
        　　``@WebListener()``没有设置初始参数的属性，所以如果要设置参数，可以在web.xml中设置：
        ```xml
            <context-param>
                <param-name>AVATAR</param-name>
                <param-value>/avatars</param-value>
            </context-param>
        ```
        ---
    +   ServletContextAttributeListener —— "监听属性改变的监听器"
        　　在ServletContext中添加、移除、替换属性时，相对应的 ``attributeAdded()`` 、``attributeRemoved()`` 、 ``attributeReplaced()`` 方法就会被调用。
        ```Java
            @WebListener()
            public class SomeContextAttrListener implements ServletContextAttributeListener{
                //代码区
                
            }
        ```
        ---
        或者是在web.xml中设置：
        ```xml
            <listener>
                <listener-class>"类的路径"</listener-class>
            </listener>
        ```

---
+  HttpSession 事件、监听器
    　　与HttpSession 相关的监听器有四个：HttpSessionListener、HttpSessionAttributeListener、HttpSessionBindingListener、HttpSessionActivationListener。 

    +   HttpSessionListener —— "生命周期监听器"
         　　在HttpSession对象初始化或结束前，会分别调用 ``sessionCreated()`` 与 ``sessionDestroyed（）`` 方法，可以通过传入的 ``HttpSessionListener`` ，使用 ``getSession()`` 取得HttpSession，以针对会话对象做出相对应的创建或结束处理操作。
        ```Java
            import java.util.EventListener;
            
            public interface HttpSessionListener extends EventListener{
                public void sessionCreated(HttpSessionEvent se);
                public void sessionDestroyed(HttpSessionEvent se);
            }
        ```

    ---
    
    +   HttpSessionAttributeListener —— "属性改变监听器"
        　　在会话对象中添加、移除、替换属性时，相对应的 ``attributeAdded()`` 、``attributeRemoved()`` 、 ``attributeReplaced()`` 方法就会被调用，并分别传入``HttpSessionBindingEvent``。
        ```Java
            import java.util.EventListener;
            public class HttpSessionAttrListener implements HttpSessionAttributeListener{
                //代码区
            }
            
        ```

        或者是在web.xml中设置：
        ```xml
            <listener>
                <listener-class>"类的路径"</listener-class>
            </listener>
            
        ```

    ---
    
    +   HttpSessionBindingListener —— "对象绑定监听器"
        　　如果有个即将加入HttpSession的属性对象，希望在设置给HttpSession成为属性或从HttpSession中移除时，可以收到HttpSession的通知，则可以让该对象实现HttpSessionBindingListener接口。
        ```Java
            import java.util.EventListener;
            
            public interface HttpSessionBindingListener extends EventListener{
                public void valueBound(HttpSessionBindingEvent event);
                public void valueUnbound(HttpSessionBindingEvent event);
            }
        ```

    ---

    +   HttpSessionActivationListener —— "对象迁移监听器"
        　　其定义了两个方法：``sessionWillPassivate()`` 与 ``sessionDidActivate()``
        　　大部分情况下都不会使用到HttpSessionActivationListener。而当使用到分布式环境时，应用程序的对象可能分散在多个JVM中，当HttpSession要从一个JVM迁移到另一个JVM时，必须在原本的JVM上序列化所有的属性对象，在这之前若属性对象有实现HttpSessionActivationListener，就会调用 sessionWillPassivate() 方法，而HttpSession迁移到另一个JVM后要对所有属性对象反序列化，就要调用 sessionDidActivate() 方法。（要可以序列化的对象必须实现Serializable接口）

---

+   HttpServletRequest 事件、监听器
    与 请求 相关的监听器有3个： ServletRequestListener 、 ServletRequestAttributeListener 、 AsyncListener

    ---

    +   ServletRequestListener —— "生命周期监听器"
    　　若要在 HttpServletRequest 对象生成或这结束时做些相对应的操作，就可以实现 ServletRequestListener 。
        ```Java
            import java.util.EventListener;
            
            public interface ServletRequestListener extends EventListener{
                public void requestDestroyed(ServletRequestEvent sre);
                public void requestInitialized(ServletRequestEvent sre);
            }
        ```

    ---
 
    +   ServletRequestAttributeListener —— "属性改变监听器"
        　　在请求对象中添加、移除、替换属性时，相对应的 ``attributeAdded()`` 、``attributeRemoved()`` 、 ``attributeReplaced()`` 方法就会被调用，并分别传入``ServletRequestAttributeListener``。
        　　ServletRequestAttributeListener 有个 ``getName()`` 方法，可以取得属性设置或者移除时指定的名称，而 ``getValue()`` 则可以取得属性设置或者移除时的对象。

---

> * 生命周期监听器和属性改变监听器都必须使用 ``@WebListener`` 或者在 ``web.xml`` 中设置，容器才会知道要加载、读取监听器相关设置

