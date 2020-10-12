#SpringBoot创建向导

###步骤
1、选择Spring Initializr，选择需要的配置即可
2、删除无关目录和文件
3、编写控制器类运行，注意@RestController包含了@Controller和@ResponseBody两个注解，因此使用这一个就行

###注意点
1、生成的项目的resources文件夹的目录结构
    static：保存静态资源，如js，css，images
    templates：保存模板页面，比如springboot使用嵌入式tomcat，默认不支持JSP页面；
        因此可以使用模板引擎如thymeleaf、freemarker等，此处就是放置模板页面的。
    application.properties:springboot的配置文件，可以在此修改原来的默认配置
