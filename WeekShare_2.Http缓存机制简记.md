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

状态代码为3位数字，200-299 的状态码表示成功，300-399 的状态码指资源重定向，400-499的状态码指客户端请求出错，500-599的状态码指服务端出错（HTTP/1.1向协议中引入了信息性状态码，范围为100-199）

2.响应头：与请求头类似，为响应报文添加了一些附加信息，常见响应头：  
**Server**： 服务器应用程序软件的名称和版本   
**Content-Type**: 响应正文的类型（是图片还是二进制字符串）  
**Content-Length**: 响应正文长度  
**Content-Charset**: 响应正文使用的编码  


##

### Http缓存 ###
Http协议是建立在TCP协议上，缓存规则信息包含在响应header中，客户端在发起一次请求时，当本地缓存数据库中没有对相应的缓存数据，需要请求服务器，服务器返回后将数据存储到缓存数据库中。Http缓存有很多规则，根据是否重新向服务器请求分为2类：**强制缓存**，**对比缓存**。强制缓存的优先级高于对比缓存。    
 
##
### 强制缓存 ###

响应header中会有两个字段来标明失效规则（Expires/Cache-Control）：  

**Expires** ：为服务端返回的到期时间，即下一次请求时，请求时间小于服务端返回的到期时间，直接使用缓存数据。HTTP 1.1 的版本，使用Cache-Control替代。  

**Cache-Control**：
**Cache-Control** 是最重要的规则。
常见的取值有**private**、**public**、**no-cache**、**max-age**，**no-store**，默认为**private**。

**private**:客户端可以缓存。  
**public**:客户端和代理服务器都可缓存。  
**max-age=x**:缓存的内容将在 x 秒后失效。  
**no-cache**:需要使用对比缓存来验证缓存数据。  
**no-store**：所有内容都不会缓存，强制缓存，对比缓存都不会触发。
 
##
### 对比缓存 ###

浏览器第一次请求数据时，服务器会将缓存标识与数据一起返回给客户端，客户端将二者备份至缓存数据库中。再次请求数据时，客户端将备份的缓存标识发送给服务器，服务器根据缓存标识进行判断，判断成功后，返回304状态码，通知客户端比较成功，可以使用缓存数据。对于对比缓存来说，缓存标识的传递是我们着重需要理解的，它在请求header和响应header间进行传递，一共分为两种标识传递：

**Last-Modified**：服务器在响应请求时，告诉浏览器资源的最后修改时间。  
**If-Modified-Since**：再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。
服务器收到请求后发现有头If-Modified-Since 则与被请求资源的最后修改时间进行比对。
若资源的最后修改时间大于If-Modified-Since，说明资源又被改动过，则响应整片资源内容，返回状态码200；若资源的最后修改时间小于或等于If-Modified-Since，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。

**Etag**：服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）。

**If-None-Match**：
再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。
服务器收到请求后发现有头If-None-Match 则与被请求资源的唯一标识进行比对，
不同，说明资源又被改动过，则响应整片资源内容，返回状态码200；
相同，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。

（Etag/If-None-Match 优先级高于 Last-Modified/If-Modified-Since）

关于http缓存的基本过程：

![avatar](https://raw.githubusercontent.com/sharetechall/resourceRepository/master/WeekShareNotes/http_cache_process.png)
## 

Share：[Nandy](https://github.com/devnns)
Summarize ：[Jiervs](https://github.com/Jiervs)


  
