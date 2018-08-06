#### Servlet

##### Servlet容器
```
Listener implements javax.servlet.ServletContextListener
    void contextInitialized(ServletContextEvent var1);
    void contextDestroyed(ServletContextEvent var1);

Filter implements javax.servlet.Filter
    void init(FilterConfig var1) throws ServletException;
    void doFilter(ServletRequest var1, ServletResponse var2, FilterChain var3) throws IOException, ServletException;
    void destroy();

Servlet implements javax.servlet.Servlet
    void init(ServletConfig var1) throws ServletException;
    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;
    void destroy();

    extends javax.servlet.GenericServlet
    public abstract void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    extends javax.servlet.http.HttpServlet [正确]
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doGet(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
```

##### 非Servlet容器
```
Interceptor implements org.springframework.web.servlet.HandlerInterceptor
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        return true;
    }
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            @Nullable ModelAndView modelAndView) throws Exception {
    }
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
            @Nullable Exception ex) throws Exception {
    }
```

##### Spring容器初始化过程
```
1.WebApplicationContext 容器初始化完成
ContextLoader Root  WebApplicationContext: initialization completed in 3496 ms

2.自定义组件以 Bean 形式注册进 Spring Context 中
自定义监听器 Bean注册  listenerRegistrationBean...
自定义过滤器 Bean注册  filterRegistrationBean...
自定义Servlet Bean注册  servletRegistrationBean...
FilterRegistrationBean  Mapping filter: 'characterEncodingFilter' to: [/*]
FilterRegistrationBean  Mapping filter: 'myFilter' to urls: [/*]
ServletRegistrationBean  Servlet myHttpServlet mapped to [/myHttpServlet]
ServletRegistrationBean  Servlet dispatcherServlet mapped to [/]

3.自定义组件的初始化
自定义监听器初始化  contextInitialized.. 
自定义过滤器初始化  init, 15 
自定义配置Bean注册  host:127.0.0.1, port=6379
SimpleUrlHandlerMapping  Mapped URL path [/**/favicon.ico]
RequestMappingHandlerAdapter Looking for @ControllerAdvice
自定义添加MVC拦截器  addInterceptors...
RequestMappingHandlerMapping  Mapped "{[/threadLocal/test],methods=[GET]}"
RequestMappingHandlerMapping  Mapped "{[/test]}"
SimpleUrlHandlerMapping  Mapped URL path [/**]

4.第一次访问MVC服务会初始化 dispatcherServlet
[Tomcat].[localhost].[/]  Initializing Spring  FrameworkServlet 'dispatcherServlet'
DispatcherServlet  FrameworkServlet 'dispatcherServlet': initialization started
DispatcherServlet  FrameworkServlet 'dispatcherServlet': initialization completed in 29 ms
自定义过滤器  doFilter, 19, /threadLocal/test
自定义MVC拦截器  preHandle
              /threadLocal/test...
              postHandle
              afterCompletion
自定义过滤器  doFilter, 21, /test
自定义MVC拦截器  preHandle
              /test...
              postHandle
              afterCompletion
自定义过滤器  doFilter, 23, /myHttpServlet
自定义Servlet  doGet...
```

##### o.s.web.servlet.DispatcherServlet 工作流程
```
1).用户向服务器发送请求，请求被Spring前端控制器 DispatcherServlet 捕获
2).DispatcherServlet 解析URL 然后调用 HandlerMapping 获得对应的Handler对象
  (包含对应的拦截器 HandlerInterceptor)
3).DispatcherServlet 根据获得的Handler对象 选择一个合适的 HandlerAdapter
  如果成功获取 HandlerAdapter，将执行拦截器的 preHandle()
4).提取Request中的模型数据，填充 Handler 入参，开始执行Handler（Controller)  
  HttpMessageConveter会做一些参数的转换(json -> 对象)
5).Handler执行完成后，向 DispatcherServlet 返回一个ModelAndView对象 
  或 Rest(Json)
6).根据返回的ModelAndView，选择一个适合的ViewResolver
  (必须是已经注册到Spring容器中的ViewResolver)返回给 DispatcherServlet。
7).ViewResolver 结合Model和View，来渲染视图
8).将渲染结果返回给客户端
```

#### 监听器 过滤器 Servlet 拦截器
```
Servlet 监听器：在容器初始化时初始化，只会执行一次contextInitialized()
Servlet 过滤器：在容器初始化时初始化，第一次执行init()
  以后每次请求都会执行doFilter()  
  作用：
    characterEncodingFilter字符编码的过滤
Servlet 本身：在第一次Servlet请求时初始化，处理每次Servlet请求
MVC拦截器 ：在第一次MVC请求时初始化，拦截每次MVC请求 (面向切面编程)

Filter是基于函数回调的，而Interceptor则是基于Java反射的。
Filter依赖于Servlet容器，而Interceptor不依赖于Servlet容器。
Filter对几乎所有的请求起作用，而Interceptor只能对action请求起作用。
Interceptor可以访问Action的上下文，值栈里的对象，而Filter不能。
在action的生命周期里，Interceptor可以被多次调用，而Filter只能在容器初始化时调用一次。
```


