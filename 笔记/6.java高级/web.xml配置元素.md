# web.xml配置元素

## <web-app>根元素

web.xml的模式文件是有sun公司定义的，每个web.xml文件的根元素<web-app>中，都必须表明使用的是哪个模式文件。其余元素也都必须放在<web-app>中。

## <context-param>上下文参数

含有一对参数名和参数值，用作应用的servlet上下文初始化参数，参数名在整个web应用中必须是唯一的，在整个生命周期中上下文初始化参数都存在，任意的servlet和jsp都可以访问。如果web.xml中不屑<context-param>配置信息，默认的路径是/WEB-INF/applicationContext.xml

## <listener>监听器

Listener是Servlet的监听器，可以监听客户端的请求，服务端的操作等。

## <servlet>

在向servlet或jsp页面制定出实话参数或定制URL时，必须首先命名servlet或jsp页面。servlet元素就是用来完成此项任务的。

## <servlet-mapping>

服务器一般为servlet提供一个缺省的URL：http://host/webAppPrefix/servlet/ServletName。但是通常会修改这个URL，以便servlet能够访问出实话参数或更容易的处理URL。