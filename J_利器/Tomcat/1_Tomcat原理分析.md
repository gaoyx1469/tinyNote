# Tomcat原理分析



### Servlet/ServletRequest/ServletResponse

* 当使用http请求时，实际使用HttpServlet作为标准，自定义的Servlet通过继承HttpServlet实现功能；同样，方法中参数也是HttpServletRequest，HttpServletResponse。
* HttpServletRequest和HttpServletResponse是接口，其具体的实现是由Servlet容器去做的，如Tomcat，采用门面模式，提供的HttpServletRequest实现类是RequestFacade，而RequestFacade中聚合了Request