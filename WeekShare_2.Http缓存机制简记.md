##Http缓存机制简记##

**Purpose** ：  
1.熟悉http协议，提高响应速度，优化体验，节省服务器开销，如静态资源。   
2.离线App的最佳实践。   
3.在线App提高性能：不常修改的资源从缓存取。

##

###Http协议的报文格式###

**HTTP请求报文主要由请求行、请求头部、请求正文3部分组成，如下：**

![avatar](https://raw.githubusercontent.com/sharetechall/resourceRepository/master/WeekShareNotes/httpReq.png)

1.请求行：由3部分组成，请求方法，**URL（Uniform Resource Locator,统一资源定位符）** 。请求方法：**(GET,HEAD,PUT,POST,TRACE,OPTIONS,DELETE以及扩展方法)** 。

2.请求头：请求头为请求报文添加一些附加信息，键值对形式为一行，常见请求头：  
**Host**：（接受请求服务器的地址，可以是IP：端口号，也可以是域名）  
**User-Agent**： (发送请求的应用程序名称)  
**Connection**: (指定与连接相关的属性，如 Connection：Keep-Alive)  
**Accept-Encoding**：（通知服务端可以发送的数据压缩格式）  
请求头部的最后一个会有空行，表示请求头部结束。

3.请求正文：（可选，GET没有请求正文）


**HTTP响应报文主要由请求行、请求头部、请求正文3部分组成，如下：**

![avatar](https://raw.githubusercontent.com/sharetechall/resourceRepository/master/WeekShareNotes/httpResp.png)

1.状态行：由3部分组成，协议版本，状态码，状态码描述。  

状态代码为3位数字，200~299的状态码表示成功，300~399的状态码指资源重定向，400~499的状态码指客户端请求出错，500~599的状态码指服务端出错（HTTP/1.1向协议中引入了信息性状态码，范围为100~199）

2.响应头：与请求头类似，为响应报文添加了一些附加信息，常见响应头： 
**Server**： 服务器应用程序软件的名称和版本   
**Content-Type**: 响应正文的类型（是图片还是二进制字符串）  
**Content-Length**: 响应正文长度  
**Content-Charset**: 响应正文使用的编码  


##

###Http通信过程（握手序列图）###
continue...  
## 
Share：[Nandy](https://github.com/devnns)
Summarize ：[Jiervs](https://github.com/Jiervs)


  
