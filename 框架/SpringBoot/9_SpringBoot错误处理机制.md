#错误处理机制

##默认效果
浏览器访问：返回默认错误页面，包括时间和状态码
客户端访问，返回JSON数据，包括时间戳和状态码，错误信息、提示消息、访问路径

###原理
参照ErrorMvcAutoConfiguration类，其中加载错误处理的默认配置
配置类中加载了以下几个bean
    DefaultErrorAttributes
        帮助在页面共享信息
    BasicErrorController
    ErrorPageCustomizer
        主要方法：registerErrorPages，其中调用了getPath方法，取值是配置的${error.path:/error}
        即出现错误之后，来到/error请求进行处理。【取代了web.xml中配置的错误响应规则】
    DefaultErrorViewResolver
首先：当系统出现4xx或5xx错误码的错误时，ErrorPageCustomizer定制错误响应规则，主要依靠其中registerErrorPages方法，来到/error请求；
其次：BasicErrorController是控制器，其实就是处理/error请求的。其中errorHtml是处理html请求的数据，返回html，error处理其它请求的数据，返回json；
    对于errorHtml方法，其调用resolveErrorView返回ModelAndView，该方法遍历容器中ErrorViewResolver列表，能解析错误的就返回
    而ErrorViewResolver列表包含DefaultErrorViewResolver这种，其类中resolve方法解析错误并返回ModelAndView。
    另：errorHtml和error方法中也调用getErrorAttributes方法返回model，该方法实际调用DefaultErrorAttributes的getErrorAttributes方法。
    具体错误返回里能有哪些错误信息，是该方法确定的。
    
    
###如何自定义错误页面
有模板引擎的情况：自己写一个错误码.html的文件，放到模板引擎文件夹下的error文件夹下就可以生效，同时，可以以4xx.html作为以4开头错误码的默认匹配项。【精确匹配优先】
没有模板引擎的情况，自己写一个错误码.html的文件，放到静态文件夹下的error文件夹下就可以生效。 
若以上都没有放置错误页面，则访问SpringBoot默认的空白页面

###如何自定义json数据
方案一：自定义一个异常处理器，使用@ControllerAdvice注解修饰，处理器中定义异常处理方法，使用@ExceptionHandler(Exception.class)注解修饰；
    方法返回一个Map集合，里面可以自定义要返回的数据。最后在方法上增加@ResponseBody注解，让map以json数据形式返回。
    这样，出现错误后都是以json数据形式返回，不再区分请求是浏览器的还是客户端的。
方案二：在方案一的基础上想要区分返回html或json，不再加@ResponseBody注解，也不返回map集合，而是返回字符串，转发到/error。
    而转发使得状态码为200，因此在方法中需要传入自己的状态码【方法参数增加HttpServletRequest，request.setAttribute("javax.servlet.error.status_code",400)】
    此时，自适应返回的html和json中还没有带入自定义的map数据
    因此需要自定义一个ErrorAttributes类继承DefaultErrorAttributes，重写getErrorAttributes方法往里添加自己想要的属性。
    对于自定义的map数据，可以放到request中，这样转发后，getErrorAttributes方法的参数里RequestAttributes对象可以获取request中的属性。
    