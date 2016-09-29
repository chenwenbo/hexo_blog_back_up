title: web_exception_handle
date: 2016-09-13 15:16:19
tags:
---

在Web应用中针对于异常情况的处理我们通常是会定义一个错误页面，然后发生错误的时候就会跳转到具体的错误页面。

### err code配置

```
<error-page>
    <error-code>404</error-code>
    <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
<error-page>
    <error-code>500</error-code>
    <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
<error-page>
    <error-code>414</error-code>
    <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
<error-page>
    <error-code>505</error-code>
    <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
<error-page>
    <error-code>400</error-code>
    <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
```

通过此种方式可以捕获到resoonse的错误返回码，然后可以跳转到错误页面。

### 异常类型配置

```
<error-page>
   <exception-type>java.lang.NullPointerException</exception-type>
   <location>/WEB-INF/jsp/errors/error.jsp</location>
</error-page>
```

此种方式我们可以捕获具体的异常，并跳转到错误页面。如果我们程序中代码写得不太好，可能会抛出各种runtimeException.
这时候仅仅配置error page就cover不到这种情况，需要上面的代码进行处理。

### SpringMVC异常处理

**HandlerExceptionResolver接口**

```
public interface HandlerExceptionResolver {
    ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex);
}
```

通过HandlerExceptionResolver接口可以实现对controller中的异常进行捕获，并给出相应的处理逻辑。

```
public class MyExceptionResolver implements HandlerExceptionResolver {

    private ExceptionLogDao exceptionLogDao;

    @Override
    public ModelAndView resolveException(HttpServletRequest request,
            HttpServletResponse response, Object handler, Exception ex) {

        // 异常处理，例如将异常信息存储到数据库
        exceptionLogDao.save(ex);

        // 视图显示专门的错误页
        ModelAndView modelAndView = new ModelAndView("errorPage");
        return modelAndView;
    }
}
```

###  SpringMVC 和 error page

默认会优先调用HandlerExceptionResolver的实现类进行处理异常，如果HandlerExceptionResolver中resolveException的返回值为null,就会默认调用error page中定义的处理方式进行处理。

### 总结
对于web项目的异常处理，我们的处理方式通常可以采用 spring异常配置 +　error page 配合进行处理，保证能够cover所有的异常情况的处理。


> [参考资料](http://cgs1999.iteye.com/blog/1547197) 