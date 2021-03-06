# HTTP超文本传输协议

HTTP超文本传输协议是TCP/IP协议的一个应用层协议

### 1）版本介绍

​		0.9
​			初始版本
​		1.0
​			信息交换过程：建立连接--发送请求信息--发出响应信息--关闭连接
​			连接过程短暂，每次只处理一个请求和响应，每一个页面的访问，浏览器与WEB服务器都要建立一次单独连接；
​			浏览器与WEB服务器之间所有的通讯都是完全独立分开的
​			性能较差
​		1.1
​			信息交换过程：建立连接--发送请求信息（多个）--发送响应信息（多个）--发出关闭连接请求--关闭连接
​			引入持久连接，在一个TCP连接上可以传送多个HTTP请求和响应
​			多个请求和响应可以重叠
​			丰富了请求头和响应头的内容
​			缺点：同一个TCP连接里面，所有的数据通信是按次序进行的。服务器只有处理完一个回应，才会进行下一个回应。要是前面的回应特别慢，后面就会有许多请求排队等着。这称为"队头堵塞"。
​		2
​			复用TCP连接，在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序一一对应，这样就避免了"队头堵塞"。这样双向的、实时的通信，就叫做多工（Multiplexing）。
​			HTTP/2 允许服务器未经请求，主动向客户端发送资源，这叫做服务器推送（server push）。
​			HTTP/2 将每个请求或回应的所有数据包，称为一个数据流（stream）。每个数据流都有一个独一无二的编号。数据包发送的时候，都必须标记数据流ID，用来区分它属于哪个数据流。另外还规定，客户端发出的数据流，ID一律为奇数，服务器发出的，ID为偶数。
​			数据流发送到一半的时候，客户端和服务器都可以发送信号（RST_STREAM帧），取消这个数据流。1.1版取消数据流的唯一方法，就是关闭TCP连接。
​			客户端还可以指定数据流的优先级。优先级越高，服务器就会越早回应。

### 2）HTTPS协议

​		HTTPS是HTTP协议的安全版本，HTTP协议的数据传输是明文的，是不安全的，HTTPS使用了SSL/TLS协议进行了加密处理。
​	HTTP协议的组成
​		请求部分
​			GET /wxisme HTTP/1.1  
​			Host: www.cnblogs.com 
​			User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.0; zh-CN; rv:1.8.1) Gecko/20061010 Firefox/2.0  
​			Accept: text/xml,application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5  
​			Accept-Language: en-us,zh-cn;q=0.7,zh;q=0.3  
​			Accept-Encoding: gzip,deflate  
​			Accept-Charset: gb2312,utf-8;q=0.7,*;q=0.7  
​			Keep-Alive: 300  
​			Proxy-Connection: keep-alive  
​			Cookie: ASP.NET_SessionId=ey5drq45lsomio55hoydzc45
​			Cache-Control: max-age=0
​			请求行
​				位于第一行，包含请求方式、请求的URI、客户端使用的协议
​				请求方式：
​					GET（默认）获取资源，带name属性的表单数据展示在地址栏，长度有限、暴露数据；
​					POST传输实体主题，通常借助HTML中的form表单设置
​					PUT传输文件
​					HEAD获得报文首部
​					DELETE删除文件
​					OPTIONS询问支持的方法
​					TRACE追踪路径
​					CONNECT要求用隧道协议连接代理
​			消息头
​				从第二行起至第一个空行
​			正文
​				第一个空行之后
​		响应部分
​			HTTP/1.1 200 OK
​			Date: Tue, 12 Jul 2016 21:36:12 GMT
​			Content-Length: 563
​			Content-Type: text/html
​			响应行
​				位于第一行，包含服务器使用的协议和状态码
​				状态码：
​					100~199:成功接收请求，需要客户端继续提交下一次请求才能完成整个处理过程
​					200~299:成功接收请求，并已完成整个处理过程
​					300~399:为完成请求，客户需进一步细化请求
​					400~499:客户端请求有错误
​					500~599:服务器端出现错误
​				常见状态码：	
​					200：正常
​					302、307：重定向，302会把POST改为GET，307不会
​					304：服务器资源未被修改，直接读取缓存即可
​					400：请求报文有语法错误
​					403：访问被禁止，没权限
​					404：资源不存在
​					408：客户端请求超时
​					500：资源报错
​					504：网关超时
​			消息头
​				从第二行起至第一个空行
​			正文
​				第一个空行之后
​		消息头：
​			请求消息头：向服务器传递附加信息
​				Accept：浏览器可接收的媒体类型MIME
​				*Accept-Charset：浏览器可支持的字符集
​				Accept-Language: 浏览器可支持的语言
​				Accept-Encoding: 浏览器能解码的数据压缩方式，如gzip
​				Host： 初始URL中的主机和端口
​				*Referer：前一页面的URL地址，主要用于防盗链
​				*Content-Type：正文的媒体类型，对应form表单的属性enctype的值，默认application/x-www-form-urlencoded
​				If-Modified-Since：通知服务器本地缓存的时间
​				User-Agent：浏览器类型
​				Content-Length：请求消息正文长度
​				Connection:取值Keep-Alive时保存连接，1.1时默认保持连接
​				*Cookie：本地缓存
​				Date：发出请求时间
​			响应消息头：
​				*Location：通知客户端，指示新的资源的位置。
​				Server：通知客户端，服务器的类型
​				*Content-Encoding：通知客户端，响应正文的数据压缩方式
​				Content-Length：返回消息正文长度
​				*Content-Type：正文的媒体类型,默认text/html后面可加【;charset=XXX】来通知浏览器解析编码
​				Last-Modified:文件最后修改时间
​				*Refresh:每过N秒浏览器自动刷新，后面可跟一个【;URL=XXXXXX】常用于登陆成功跳转
​				*Content-Disposition：通知客户端以下载的方式打开资源，如Content-Disposition:attachment;filename=XXX
​				*Set-Cookie：通知客户端设置cookie
​				以下三个一起用：
​				*Expires：网页的有效时间，单位为毫秒，-1代表通知客户端不要缓存
​				*Cache-Control：no-cache （1.1）通知客户端不要缓存
​				*Pragma：no-cache （1.0）通知客户端不要缓存
​				Date：服务器响应时间