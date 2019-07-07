### 拦截器与过滤器的区别

#### 过滤器：

​	依赖于servlet容器。在实现上基于函数回调，可以对几乎所有请求进行过滤。但是缺点是一个过滤器实例只能在容器初始化时调用一次。使用过滤器的目的是用来做一些过滤操作，获取我们想要获取的数据。

**比如：在过滤器中修改字符编码；在过滤器中修改HttpServletRequest的一些参数，包括：过滤低俗文字、危险字符等**

#### 拦截器：

​	依赖于web框架，在SpringMVC中就是依赖于SpringMVC框架。在实现上基于Java的反射机制，属于面向切面编程（AOP）的一种运用。由于拦截器是基于web框架的调用，因此可以使用Spring的依赖注入（DI）进行一些业务操作，同时一个拦截器实例在一个controller生命周期之内可以多次调用。但是缺点是只能对controller请求进行拦截，对其他的一些比如直接访问静态资源的请求则没办法进行拦截处理

#### 过滤器和拦截器的区别：

​		①拦截器是基于java的反射机制的，而过滤器是基于函数回调。

​		②拦截器不依赖与servlet容器，过滤器依赖与servlet容器。

​		③拦截器只能对action请求起作用，而过滤器则可以对几乎所有的请求起作用。

​		④拦截器可以访问action上下文、值栈里的对象，而过滤器不能访问。

​		⑤在action的生命周期中，拦截器可以多次被调用，而过滤器只能在容器初始化时被调用一次。

​		⑥拦截器可以获取IOC容器中的各个bean，而过滤器就不行，这点很重要，在拦截器里注入一个service，可以调用业务逻辑。

#### 过滤器主要代码实现：

```Java
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {    
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    }
    @Override
    public void destroy() {
    }
```

#### 拦截器主要实现代码：

```Java
  @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //System.out.println("拦截器执行了----前1");
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        //System.out.println("拦截器执行了----后1");
        //request.getRequestDispatcher("/error.jsp").forward(request,response);
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        //System.out.println("拦截器执行了----最后1");
    }
```

#### 使用场景

1. ##### 过滤器

   过滤器（Filter）：当你有一堆东西的时候，你只希望选择符合你要求的某一些东西。定义这些要求的工具，就是过滤器。（理解：就是一堆字母中取一个B）

2. ##### 拦截器

   拦截器（Interceptor）：在一个流程正在进行的时候，你希望干预它的进展，甚至终止它进行，这是拦截器做的事情。（理解：就是一堆字母中，干预他，通过验证的少点，顺便干点别的东西）