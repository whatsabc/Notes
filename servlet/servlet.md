# 1 HTTP协议

## 1 HTTP简介

HTTP（HyperText Transfer Protocol）即超文本传输协议，是一种详细规定了浏览器和万维网服务器之间互相通信的规则，它是万维网交换信息的基础，它允许将HTML（超文本标记语言）文档从Web服务器传送到Web浏览器。

HTTP协议目前最新版的版本是1.1，HTTP是一种无状态的协议，无状态是指Web浏览器与Web服务器之间不需要建立持久的连接，这意味着当一个客户端向服务器端发出请求，然后Web服务器返回响应（Response），连接就被关闭了，在服务器端不保留连接的有关信息。也就是说，HTTP请求只能由客户端发起，而服务器不能主动向客户端发送数据。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

## 2 HTTP协议概述

HTTP是一个客户端终端（用户）和服务器端（网站）请求和应答的标准（TCP）。通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80）。我们称这个客户端为用户代理程序（user agent）。应答的服务器上存储着一些资源，比如HTML文件和图像。我们称这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个“中间层”，比如代理服务器、网关或者隧道（tunnel）。

尽管TCP/IP协议是互联网上最流行的应用，HTTP协议中，并没有规定必须使用它或它支持的层。事实上，HTTP可以在任何互联网协议上，或其他网络上实现。HTTP假定其下层协议提供可靠的传输。因此，任何能够提供这种保证的协议都可以被其使用。因此也就是其在TCP/IP协议族使用TCP作为其传输层。

通常，由HTTP客户端发起一个请求，创建一个到服务器指定端口（默认是80端口）的TCP连接。HTTP服务器则在那个端口监听客户端的请求。一旦收到请求，服务器会向客户端返回一个状态，比如"HTTP/1.1 200 OK"，以及返回的内容，如请求的文件、错误消息、或者其它信息。

## 3 HTTP工作原理

HTTP协议工作于客户端-服务端架构上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。

Web服务器有：Apache服务器，IIS服务器（Internet Information Services）等。

Web服务器根据接收到的请求后，向客户端发送响应信息。

HTTP默认端口号为80，但是你也可以改为8080或者其他端口。

**以下是 HTTP 请求/响应的步骤：**

1. 客户端连接到Web服务器

一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如，[http://www.luffycity.com](http://www.luffycity.com/)。

2. 发送HTTP请求

通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成。

3. 服务器接受请求并返回HTTP响应

Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。

4. 释放连接TCP连接

若connection模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接;若connection模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;

5. 客户端浏览器解析HTML内容

客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

1. 浏览器向DNS服务器请求解析该URL中的域名所对应的IP地址;
2. 解析出IP地址后，根据该IP地址和默认端口80，和服务器建立TCP连接;
3. 浏览器发出读取文件(URL中域名后面部分对应的文件)的HTTP请求，该请求报文作为TCP三次握手的第三个报文的数据发送给服务器;
4. 服务器对浏览器请求作出响应，并把对应的html文本发送给浏览器;
5. 释放TCP连接;
6. 浏览器将该html文本并显示内容; 

**HTTP注意事项：**

- HTTP是无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
- HTTP是媒体独立的：这意味着，只要客户端和服务器知道如何处理的数据内容，任何类型的数据都可以通过HTTP发送。客户端以及服务器指定使用适合的MIME-type内容类型。
- HTTP是无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。


## 4 HTTP请求与响应

HTTP遵循请求（Request）/应答（Response）模型，Web浏览器向Web服务器发送请求时，Web服务器处理请求并返回适当的应答。

### 4.1 HTTP请求格式

```
POST  /test.php  HTTP/1.1             //请求行
HOST：www.test.com                    //请求头
User-Agent：Mozilla/5.0 (windows NT 6.1;rv：15.0)Gecko/20100101 Firefox/15.0
Accept-Language:zh-CN
Connection:Keep-Alive
Cookie:----
									//空白行，代表请求头结束
Username=admin&password=admin       //请求正文
```

HTTP请求包括三部分，分别是请求行（请求方法）、请求头（消息报头）请求空行和请求正文。

- HTTP请求第一行为请求行，由三部分组成，第一部分说明了该请求时POST请求，第二部分是一个斜杠（/login.php），用来说明请求是该域名根目录下的login.php，第三部分说明使用的是HTTP1.1版本。
- 请求空行（空行）
- HTTP请求第二行至空白行为请求头（也被称为消息头）。其中，HOST代表请求主机地址，User-Agent代表浏览器的标识，请求头由客户端自行设定。
- HTTP请求第三行为请求正文，请求正文是可选的，它最常出现在POST请求方式中。

### 4.2 HTTP请求方法

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。

HTTP1.0 定义了三种请求方法： GET, POST 和HEAD方法。HTTP1.1 新增了五种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。


| 方法  | 描述   |
| ---- | ---- |
|GET	|请求指定的页面信息，并返回实体主体。|
|HEAD|	类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头|
|POST|	向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改。|
|PUT|	从客户端向服务器传送的数据取代指定的文档的内容。|
|DELETE|	请求服务器删除指定的页面。|
|CONNECT|	HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。|
|OPTIONS|	允许客户端查看服务器的性能。|
|TRACE|	回显服务器收到的请求，主要用于测试或诊断。|
|PATCH|	是对 PUT 方法的补充，用来对已知资源进行局部更新 。|

GET、POST、HEAD、PUT请求：

- GET：GET方法用于获取请求页面的指定信息。如果请求资源为动态脚本（非HTML），那么返回文本是Web容器解析后的HTML源代码。GET请求没有消息主体，**请求参数在请求行中**，因此在消息头后的空白行是没有其他数据。
- POST：POST方法也与GET方法相似，但最大的区别在于，GET方法没有请求内容，而POST是有请求内容的，**请求参数在请求体中**。
- HEAD：这个请求的功能与GET请求相似，不同之处在于服务器不会再其响应中返回消息主体，因此，这种方法可用于检查某一资源在向其提交GET请求前是否存在。
- PUT：PUT方法用于请求服务器把请求中的实体存储在请求资源下，如果请求资源已经在服务器中存在，那么将会用此请求中的数据替换原先的数据。向服务器上传指定的资源。

### 4.3 HTTP响应格式

```
HTTP/1.1 200 OK   					  //响应行
Date: Sun,15 Nov 2015 11:02:04 GMT    //响应头
Server: bfe/1.0.8.9
Content-Length: 2605
Content-Type: application/javascript
Cache-Control: max-age=315360000
Expires: Fri, 13 Jun 2025 09:54:00 GMT
Content-Encoding: gzip
Set-Cookie: H_PS_PSSID=2022_1438_1944_1788; path=/; domain=test.com
Connection: keep-alive
					      //空白行，代表响应头结束
<html>
<head><title> Index.html </title></head>  //响应正文消息主题
```


HTTP响应的第一行为响应行，其中有HTTP版本（HTTP/1.1）、状态码（200）以及消息“OK”。

第二行至末尾的空白行为响应头，由服务器向客户端发送。消息头之后是响应正文，是服务器向客户端发送的HTML数据。

**HTTP消息头：**

请求头：请求头只出现在HTTP请求中，请求报头允许客户端向服务端传递请求的附加信息和客户端自身信息。

响应头：响应头是服务器根据请求向客户端发送的HTTP头。

**HTTP请求头：**

- Host请求报头域主要用于指定被请求资源的Internet主机和端口。
- User-Agent请求报头域允许客户端将它的操作系统、浏览器和其他属性告诉服务器。
- Referer包含一个URL，代表当前访问URL的上一个URL，也就是说，用户是从什么地方来到本页面。当前请求的原始URL地址。
- Cookie是非常重要的请求头，常用来表示请求者的身份等。
- Accept这个消息头用于告诉服务器客户端愿意接受那些内容，比如图像类，办公文档格式等等。

HTTP响应头信息：

|应答头|说明|
|---|---|
|Allow|	服务器支持哪些请求方法（如GET、POST等）。|
|Content-Encoding|文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。|
|Content-Length|表示内容长度。|
|Content-Type|**表示后面的文档属于什么MIME类型。**Servlet默认为text/plain，但通常需要显式地指定为text/html。|
|Date|	当前的GMT时间。|
|Expires|应该在什么时候认为文档已经过期，从而不再缓存它？|
|Last-Modified|	**文档的最后改动时间。**客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。|
|Location|**表示客户应当到哪里去提取文档。**Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。|
|Refresh|表示浏览器应该在多少时间之后刷新文档，以秒计。注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。|
|Server|服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。|
|Set-Cookie|设置和页面关联的Cookie。|
|WWW-Authenticate|客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。|


## 5 HTTP状态与会话

### 5.1 HTTP状态码

当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。

HTTP状态码的英文为HTTP Status Code。

**五种状态码：**

- 1xx：信息提示，表示请求已被成功接收，继续处理。
- 2xx：请求被成功提交。
- 3xx：客户端被重定向到其他资源。
- 4xx：客户端错误状态码，格式错误或者不存在资源。
- 5xx：描述服务器内部错误。

**常见的状态码描述如下：**

200：客户端请求成功，是最常见的状态。

302：<"HTTP/1.1 302Found（or Moved Temporarily）" 重定向

303：<"HTTP/1.1 303See Other"

304 Not Modified：告诉客户端请求的这个资源至上次取得后，并没有更改，直接用本地的缓存

404：请求资源不存在，是最常见的状态。

400：客户端请求有语法错误，不能被服务器所理解。

401：请求未经授权。

403：服务器收到请求，但是拒绝提供服务。

500：服务器内部错误，是最常见的状态。

503：服务器当前不能处理客户端的请求。

### 5.2 会话与会话状态简介

WEB应用中的会话是指一个客户端浏览器与WEB服务器之间连续发生的一系列请求和响应过程。

WEB应用的会话状态是指WEB服务器与浏览器在会话过程中产生的状态信息，借助会话状态，WEB服务器能够把属于同一会话中的一系列的请求和响应过程关联起来。

如何实现有状态的会话：

某个用户从网站的登录页面登入后，在进入购物页面购物时，负责处理购物请求的服务器程序必须知道处理上一次请求的程序所得到的用户信息。

HTTP协议是一种无状态的协议，WEB服务器本身不能识别出哪些请求是同一个浏览器发出的 ，浏览器的每一次请求都是完全孤立的。

WEB服务器端程序要能从大量的请求消息中区分出哪些请求消息属于同一个会话，即能识别出来自同一个浏览器的访问请求，这需要浏览器对其发出的每个请求消息都进行标识，属于同一个会话中的请求消息都附带同样的标识号，而属于不同会话的请求消息总是附带不同的标识号，这个标识号就称之为会话ID（SessionID）。

会话ID可以通过一种称之为Cookie的技术在请求消息中进行传递，也可以作为请求URL的附加参数进行传递。会话ID是WEB服务器为每客户端浏览器分配的一个唯一代号，它通常是在WEB服务器接收到某个浏览器的第一次访问时产生，并且随同响应消息一道发送给浏览器。

会话过程由WEB服务器端的程序开启，一旦开启了一个会话，服务器端程序就要为这个会话创建一个独立的存储结构来保存该会话的状态信息，同一个会话中的访问请求都可以且只能访问属于该会话的存储结构中的状态信息。

## 6 URL

超文本传输协议（HTTP）的统一资源定位符将从因特网获取信息的五个基本元素包括在一个简单的地址中：

- 传送协议。
- 层级URL标记符号(为[//],固定不变)
- 访问资源需要的凭证信息（可省略）
- 服务器。（通常为域名，有时为IP地址）
- 端口号。（以数字方式表示，若为HTTP的默认值“:80”可省略）
- 路径。（以“/”字符区别路径中的每一个目录名称）
- 查询。（GET模式的窗体参数，以“?”字符为起点，每个参数以“&”隔开，再以“=”分开参数名称与数据，通常以UTF8的URL编码，避开字符冲突的问题）
- 片段。以“#”字符为起点

以http://www.luffycity.com:80/news/index.html?id=250&page=1 为例, 其中：

- http，是协议；
- www.luffycity.com ，是服务器；
- 80，是服务器上的默认网络端口号，默认不显示；
- /news/index.html，是路径（URI：直接定位到对应的资源）；
- ?id=250&page=1，是查询。

大多数网页浏览器不要求用户输入网页中“http://”的部分，因为绝大多数网页内容是超文本传输协议文件。同样，“80”是超文本传输协议文件的常用端口号，因此一般也不必写明。一般来说用户只要键入统一资源定位符的一部分（www.luffycity.com:80/news/index.html?id=250&page=1 ）就可以了。

由于超文本传输协议允许服务器将浏览器重定向到另一个网页地址，因此许多服务器允许用户省略网页地址中的部分，比如www。从技术上来说这样省略后的网页地址实际上是一个不同的网页地址，浏览器本身无法决定这个新地址是否通，服务器必须完成重定向的任务。

## 7 Cookie技术

Cookie是一种在客户端保持HTTP状态信息的技术，它好比商场发放的优惠卡。

Cookie是在浏览器访问WEB服务器的某个资源时，由WEB服务器在HTTP响应消息头中附带传送给浏览器的一片数据，WEB服务器传送给各个客户端浏览器的数据是可以各不相同的。

一旦WEB浏览器保存了某个Cookie，那么它在以后每次访问该WEB服务器时，都应在HTTP请求头中将这个Cookie回传给WEB服务器。

> Cookie技术最先被Netscape公司引入到Navigator浏览器中。现在，绝大多数浏览器都支持Cookie，或者至少兼容Cookie技术的使用。**Cookie是一小段文本信息，**伴随着用户请求和页面在Web服务器和浏览器之间传递。Cookie包含每次用户访问站点时Web应用程序都可以读取的信息。Cookie只是一段文本，所以它只能保存字符串。
>

WEB服务器通过在HTTP响应消息中增加Set-Cookie响应头字段将Cookie信息发送给浏览器，浏览器则通过在HTTP请求消息中增加Cookie请求头字段将Cookie回传给WEB服务器。

一个Cookie只能标识一种信息，它至少含有一个标识该信息的名称（NAME）和设置值（VALUE）。

一个WEB站点可以给一个WEB浏览器发送多个Cookie，一个WEB浏览器也可以存储多个WEB站点提供的Cookie。

浏览器一般只允许存放300个Cookie，每个站点最多存放20个Cookie，每个Cookie的大小限制为4KB。

### 7.1 Cookie功能特点

- 存储于浏览器头部/传输于HTTP头部
- 写时带属性，读时无属性
- HTTP头中Cookie: user=bob; cart=books;
- 属性 name/value/expire/domain/path/httponly/secure/…
- 由三元组[name,domain,path]唯一确定cookie

### 7.2 Set-Cookie2响应字段

Set-Cookie2头字段用于指定WEB服务器向客户端传送的Cookie内容，但是按照Netscape规范实现Cookie功能的WEB服务器，使用的是Set-Cookie头字段，两者的语法和作用类似。

Set-Cookie2头字段中设置的cookie内容是具有一定格式的字符串，它必须以Cookie的名称和设置值开头，格式为“名称=值”，后面可以加上0个或多个以分号（;）和空格分隔的其它可选属性，属性格式一般为“属性名=值”。

> 举例：
>
> Set-Cookie2: user=hello; Version=1; Path=/

除了“名称=值”对必须位于最前面外，其它的可选属性的先后顺序可以任意。

Cookie的名称只能由普通的英文ASCII字符组成，浏览器不用关心和理解Cookie的值部分的意义和格式，只要WEB服务器能理解值部分的意义就行。

大多数现有的WEB服务器都是采用某种编码方式将值部分的内容编码成可打印的ASCII字符，RFC 2965规范中没有明确限定编码方式。

Cookie请求字段：

- Cookie请求头字段中的每个Cookie之间用逗号（,）或分号（;）分隔。
- 在Cookie请求头字段中除了必须有“名称=值”的设置外，还可以有Version、Path、Domain、Port等几个属性。
- 在Version、Path、Domain、Port等属性名之前，都要增加一个“$”字符作为前缀。
- Version属性只能出现一次，且要位于Cookie请求头字段设置值的最前面，如果需要设置某个Cookie信息的 Path、Domain、Port等属性，它们必须位于该Cookie信息的“名称=值”设置之后。•浏览器使用Cookie请求头字段将Cookie信息回送给WEB服务器。
- 多个Cookie信息通过一个Cookie请求头字段回送给WEB服务器。
- 浏览器根据下面的几个规则决定是否发送某个Cookie信息：

  - 请求的主机名是否与某个存储的Cookie的Domain属性匹配；

  - 请求的端口号是否在该Cookie的Port属性列表中；

  - 请求的资源路径是否在该Cookie的Path属性指定的目录及子目录中；

  - 该Cookie的有效期是否已过。

- Path属性指向子目录的Cookie排在Path属性指向父目录的Cookie之前。

举例：Cookie: $Version=1; Course=Java; $Path=/hello/lesson; Course=vc; $Path=/hello

### 7.3 Cookie安全属性

- secure属性

当设置为true时，表示创建的Cookie会被以安全的形式向服务器传输，也就是只能在HTTPS连接中被浏览器传递到服务器端进行会话验证，如果是HTTP连接则不会传递该信息，所以不会被窃取到Cookie的具体内容。

- HttpOnly属性

如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击。

secure属性是防止信息在传递的过程中被监听捕获后信息泄漏，HttpOnly属性的目的是防止程序获取cookie后进行攻击。

这两个属性并不能解决cookie在本机出现的信息泄漏的问题(FireFox的插件FireBug能直接看到cookie的相关信息)。

## 8 Session技术

### 8.1 Session简介

使用Cookie和附加URL参数都可以将上一次请求的状态信息传递到下一次请求中，但是如果传递的状态信息较多，将极大降低网络传输效率和增大服务器端程序处理的难度。

Session技术是一种将会话状态保存在服务器端的技术 ，它可以比喻成是医院发放给病人的病历卡和医院为每个病人保留的病历档案的结合方式 。

客户端需要接收、记忆和回送Session的会话标识号，Session可以且通常是借助Cookie来传递会话标识号。

### 8.2 Session跟踪机制

Servlet API规范中定义了一个HttpSession接口，HttpSession接口定义了各种管理和操作会话状态的方法。

HttpSession对象是保持会话状态信息的存储结构，一个客户端在WEB服务器端对应一个各自的HttpSession对象。

WEB服务器并不会在客户端开始访问它时就创建HttpSession对象，只有客户端访问某个能与客户端开启会话的Servlet程序时，WEB应用程序才会创建一个与该客户端对应的HttpSession对象。

WEB服务器为HttpSession对象分配一个独一无二的会话标识号，然后在响应消息中将这个会话标识号传递给客户端。客户端需要记住会话标识号，并在后续的每次访问请求中都把这个会话标识号传送给WEB服务器，WEB服务器端程序依据回传的会话标识号就知道这次请求是哪个客户端发出的，从而选择与之对应的HttpSession对象。

WEB应用程序创建了与某个客户端对应的HttpSession对象后，只要没有超出一个限定的空闲时间段，HttpSession对象就驻留在WEB服务器内存之中，该客户端此后访问任意的Servlet程序时，它们都使用与客户端对应的那个已存在的HttpSession对象。

HttpSession接口中专门定义了一个setAttribute方法来将对象存储到HttpSession对象中，还定义了一个getAttribute方法来检索存储在HttpSession对象中的对象，存储进HttpSession对象中的对象可以被属于同一个会话的各个请求的处理程序共享。

Session是实现网上商城的购物车的最佳方案，存储在某个客户Session中的一个集合对象就可充当该客户的一个购物车。

### 8.3 Session超时管理

WEB服务器无法判断当前的客户端浏览器是否还会继续访问，也无法检测客户端浏览器是否关闭，所以，即使客户已经离开或关闭了浏览器，WEB服务器还要保留与之对应的HttpSession对象。

随着时间的推移而不断增加新的访问客户端，WEB服务器内存中将会因此积累起大量的不再被使用的HttpSession对象，并将最终导致服务器内存耗尽。

WEB服务器采用“超时限制”的办法来判断客户端是否还在继续访问，如果某个客户端在一定的时间之内没有发出后续请求，WEB服务器则认为客户端已经停止了活动，结束与该客户端的会话并将与之对应的HttpSession对象变成垃圾。

如果客户端浏览器超时后再次发出访问请求，WEB服务器则认为这是一个新的会话的开始，将为之创建新的HttpSession对象和分配新的会话标识号。

会话的超时间隔可以在web.xml文件中设置，其默认值由Servlet容器定义。

```xml
<session-config>
	<session-timeout>30</session-timeout>
</session-config>
```

### 8.4 利用Cookie实现Session跟踪

如果WEB服务器处理某个访问请求时创建了新的HttpSession对象，它将把会话标识号作为一个Cookie项加入到响应消息中，通常情况下，浏览器在随后发出的访问请求中又将会话标识号以Cookie的形式回传给WEB服务器。

WEB服务器端程序依据回传的会话标识号就知道以前已经为该客户端创建了HttpSession对象，不必再为该客户端创建新的HttpSession对象，而是直接使用与该会话标识号匹配的HttpSession对象，通过这种方式就实现了对同一个客户端的会话状态的跟踪。

### 8.5 利用URL重写实现Session跟踪

Servlet规范中引入了一种补充的会话管理机制，它允许不支持Cookie的浏览器也可以与WEB服务器保持连续的会话。这种补充机制要求在响应消息的实体内容中必须包含下一次请求的超链接，并将会话标识号作为超链接的URL地址的一个特殊参数。

将会话标识号以参数形式附加在超链接的URL地址后面的技术称为URL重写。如果在浏览器不支持Cookie或者关闭了Cookie功能的情况下，WEB服务器还要能够与浏览器实现有状态的会话，就必须对所有可能被客户端访问的请求路径（包括超链接、form表单的action属性设置和重定向的URL）进行URL重写。

HttpServletResponse接口中定义了两个用于完成URL重写方法：

- encodeURL方法
- encodeRedirectURL方法

### 8.6 Cookie和Session的不同

session和cookies同样都是针对单独用户的变量（或者说是对象好像更合适点），不同的用户在访问网站的时候 都会拥有各自的session或者cookies，不同用户之间互不干扰。

他们的不同点是：

1. 存储位置不同

session在服务器端产生，比较安全，但是如果session较多则会影响性能

cookies在客户端产生，安全性稍弱

2. 生命周期不同

session生命周期在指定的时间（如20分钟）到了之后会结束，不到指定的时间，也会随着浏览器进程的结束而结束。

cookies默认情况下也随着浏览器进程结束而结束，但如果手动指定时间，则不受浏览器进程结束的影响。

cookie和session的区别：

1. cookie数据存放在客户的浏览器上，session数据放在服务器上。

2. cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session
   
3. session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能

4. 考虑到减轻服务器性能方面，应当使用cookie

单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的cookie不能3K。

# 2 容器

## 1 什么是容器？

**servlet没有main()方法。它们受控于另一个Java应用，这个Java应用称为容器。**

Tomcat就是这样一个容器，Web服务器应用（如Apache）得到一个只想servlet的请求（而不是其他请求，如请求一个普通的静态HTML页面）时，服务器不是把这个请求交给servlet本身，而是交给部署该servlet的容器。要由容器向servlet提供HTTP请求和响应，而且要由容器调用servlet的方法（如doPost()或doGet()）

## 2 容器能提供什么？

我们知道，要由容器来管理和运行servlet，但是为什么要这样呢？由此会带来一些额外的开销，这样值得吗？

- 通信支持

利用容器提供的方法，你能轻松地让servlet与Web服务器对话。你不用自己建立ServertSocket监听某个端口、创建流等等。容器知道自己与web服务器之间的协议，所以你的servlet不必担心Web服务器（如Apache）和你自己的Web代码之间的API。你要考虑的只是如何在servlet中实现业务逻辑（如接受在线商店的订单）

- 生命周期管理

容器控制着servlet的生与死。它会负责加载类、实例化和初始化servlet、调用servlet方法，以及使 servlet实例能够被垃圾回收。有了容器的控制你就不用太多地考虑资源管理了

- 多线程支持

容器会自动地为它接收的每个servlet请求创建一个新的Java线程。针对客户的请求，如果servlet已经运行完相应的HTTP服务方法，这个线程就会结束（也就是说，死掉），这并不是说你不用考虑线程安全性，你还是会遇到同步问题的。不过，由服务器创建和管理多个线程来处理多个请求，这样确实能让你少做很多工作

- 声明方式实现安全

利用容器，可以使用XML部署描述文件来配置（和修改）安全性，而不必将其硬编码写到servlet（或其他）类代码中，想像一下，不用去改你的Java源文件，也不用重新编译，你就能管理和修改安全性配置，这是多么诱人呀！

- JSP支持

容器还可以把JSP翻译成Java

## 3 容器处理请求的过程

1. 用户点击一个链接，指向一个servlet而不是一个静态页面
2. 容器看出来这个请求要的是一个servlet，所以容器创建两个对象HttpServletResponse和HttpServletRequest
3. 容器根据请求中的URL找到正确的servlet，为这个请求创建或分配一个线程，并把请求和响应对象传递给这个servlet线程。
4. 容器调用servlet的service()方法，根据请求的不同类型，service()方法会调用doGet()或doPost()方法。现在我们假设请求是一个HTTP GET请求
5. doGet()方法生成动态页面，并把这个页面写入到响应对象中。要记住，容器还有响应对象的一个引用。
6. 线程结束，容器把响应对象转换为一个HTTP响应，把它发给客户，然后删除请求和响应对象。

**下面看一个servlet实例**

```java
// 导入必需的 java 库
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

// 扩展HttpServlet类（大多数servlet都会扩展）
public class HelloWorld extends HttpServlet {
 
	private String message;

	public void init() throws ServletException {
		// 执行必需的初始化
		message = "Hello World";
	}
	//在实际中，大多数servlet都会覆盖doGet()或doPost()方法
	public void doGet(HttpServletRequest request, HttpServletResponse response)
		throws ServletException, IOException {//servlet从这里拿到容器所创建请求和响应对象的引用
		//设置响应内容类型
		response.setContentType("text/html");

		//实际的逻辑是在这里
        PrintWriter out = response.getWriter();//在servlet从容器得到的响应对象中，可以拿到一个PrintWriter。使用这个PrintWriter能够将HTML文本输出到响应对象。
		out.println("<h1>" + message + "</h1>");
	}
  
	public void destroy() {
		// 什么也不做
	}
}
```

service()方法是从servlet从HttpServlet继承来的，HttpServlet则是从GenericServlet继承，而GenericServlet自己又是从……这个类层次结构会在Servlet一节中解释了。

## 4 一个servlet可以有3个名字

显然， servlet有一个文件路径名，比如classes/registration/ SignSeRvlet.class（具体类文件的路径）。开发servlet类的人要选择类名（以及包名，其中定义了部分目录结构），服务器上的位置则决定了完整的路径名。不过，部署servlet的任何人都可以给它起一个特殊的部署名。部署名只是一个秘密的内部名，不必非得与类名或文件名相同。可以是servlet类名（registra-tion.SignUpServlet，也可以是类文件的相对路径（classes/registration/SignUpServlet.class），不过还可以是完全不同的一个名字（比如EnrollScrvlet）。

最后，servlet还有一个公共的URL名，这是客户所知道的名字。换句话说，这个名宇要写在HTML中，这样当用户点击一个指向该 servlet的链接时，就可以把这个公共URL名放在HTTP请求中发送给服务器

用部署文件把URL映射到servlet

默认情况下，Servlet 应用程序位于路径 `<Tomcat-installation-directory>/webapps/ROOT `下，且类文件放在 `<Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes`中。

如果您有一个完全合格的类名称com.myorg.MyServlet**，那么这个servlet类必须位于WEB-INF/classes/com/myorg/MyServlet.class中。

现在，让我们把 HelloWorld.class 复制到` <Tomcat-installation-directory>/webapps/ROOT/WEB-INF/classes` 中，并在位于 `<Tomcat-installation-directory>/webapps/ROOT/WEB-INF/`的**web.xml**文件中创建以下条目：

```xml
<web-app>      
	<servlet>
		<servlet-name>HelloWorld</servlet-name>
		<servlet-class>HelloWorld</servlet-class>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>HelloWorld</servlet-name>
		<url-pattern>/HelloWorld</url-pattern>
	</servlet-mapping>
</web-app>  
```

## 5 使用部署描述文件将URL映射到servlet

其中，用于URL映射的两个部署描述文件元素如下：

```
<servlet>
	把内部名映射到完全限定类名
<servlet-mapping>
	把内部名映射到公共URL名(用户真正看到的servlet名，虚构的)
```

`<servlet>`元素告诉容器一个特定的Web应用有哪些类文件

`<servlet-mapping>`回答这样一个问题，对于请求的这个URL，因该调用哪个URL

# 3 servlet

## 1 servlet受容器的控制

在第2章，我们了解到容器全盘控制着servlet的一生，它会创建请求和响应对象，为servlet创建或分配一个线程，并调用servlet的service()方法，把请求和响应对象的引用作为参数传递给servlet，下面是一个简单的回顾

1. 用户点击一个链接，链接的URL指向一个servlet

2. 容器“看出”这个请求指向个servlet，所以容器创建两个对象

- HttpsSrvletRespons
- HttpServletRequest

3. 容器根据请求中的URL找到正确的servlet，为这个请求创建或分配一个线程，并调用servlet的service方法，把请求和响应对象作为参数传递给它。
4. service()方法根据客户发出的HTTP方法（GET,POST等），确定要调用那个servlet方法。（客户发出了一个HTTP GET请求，所以service()方法会把调用servlet的doGet()方法，把请求和响应对象作为参数传递给它）

5. servlet使用响应对象将响应写给客户。响应通过容器传回
6. service()方法结束，所以线程要么撤销，要么返回到容器管理的一个线程池。请求和响应对象引用已经出了作用域，所以这些对象也没有意义了（可以垃圾回收）。
7. 客户得到响应

## 2 Servlet的生命周期

Servlet 生命周期可被定义为从创建直到毁灭的整个过程。以下是Servlet遵循的过程：

- Web容器加载AServlet.class
- Web容器初始化servlet（构造函数运行）
- Servlet通过调**init()**方法进行初始化。
- Servlet调用**service()**方法来处理客户端的请求。（service()的一生主要都在这里渡过）（每个请求都在一个单独的线程中运行）（确定HTTP方法GET或POST，并在servlet上调用对应的方法doGet(),doPost()）
- Servlet通过调用**destroy()**方法终止（结束）。
- 最后，Servlet是由JVM的垃圾回收器进行垃圾回收的。

servlet的生命周期很简单：只有一个主要的状态——初始化。如果servlet没有初始化，则要么正在初始化（运行其构造函数或init()方法），正在撤销（运行其destory()方法），要么就是还不存在。

## 3 servlet是怎样实现的

首先有一个Servlet接口（javax.servlet.Servlet），所有的servlet都有这五个方法（其中3个注释了的方法是生命周期方法）

```java
//javax.servlet.Servlet
interface Servlet{
	service(ServletRequest,ServletResponse);//生命周期方法
	init(ServletConfig)//生命周期方法
	destroy;//生命周期方法
	getServetConfig();
	getServletInfo();
}
```

GenericServlet类（javax.servlet.GenericServlet）

Cenenieserulet是一个抽象类，**它实现了你需要的大部分本servlet方法（包括Servlet接口中的方法）。**大多数servlet的“servlet行为”都来自这个类。

```java
class GenericServlet {
	service(ServletRequest,ServletResponse)
	init(ServletConfig);
	destroy();
	Servletconfig();
	Servletlnfo();
	getinitParameter(String);
	getlnitParameter();
	getservletContext();
	log(String);
	log(String, Throwable);
}
```

HttpServlet类（javax.servlet.http.HttpServlet）

HttpServlet（也是一个抽象类），**实现了一个service()方法来反映servlet的HTTP特性**——service()方法不仅仅可以配一殷的servlet请求和响应，还可以取HTTP特定的请求和应作为参数。

```java
abstract class HttpServlet{
	service(HttpServletRequest,HttpServletResponse);//这个方法很重要
	service(ServletRequest, ServletResponse);
	doGet(httpServleTRequest, HttpServletResponse);
	doPost(HttpServletRequest, httpServletResponse);
	doHead(HttpServletRequest, HttpServletResponse);
	doOptions(httpServletRequest,HttpServletResponse);
	doPut(HttpServletRequest, HttpServletresponse);
	doTrace(httpServletRequest,HttpServletResponse);
	doDelete(httpServletRequest,HttpServletresponse);
	getLastModified(httpServletRequest);
}
```

MyServlet类（com.xxxx）

大多数servlet行为都由超类方法处理。你要做的只是覆盖所需的HTTP方法。

```java
class MyServlet {
	doPost(HttpServletRequest,HttpServletResponse);
	myBizMethod();
}
```

## 4 生命周期中的三个重要时刻

| 方法                 | 何时调用                                                     | 作用                                                         | 是否覆盖                                                     |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| init()               | servlet实例创建后，并在servlet能为客户提供服务之前，容器要对servlet调用init() | 使你在servlet处理客户请求之前有机会对其初始化                | 有可能。如果有初始化代码（比如得到一个数据库连接，或向其他对象注册），就要覆盖servlet类中的init()方法 |
| service()            | 第一个客户请求到来时，容器会开始一个新线程，或者从线程池分配一个线程，并调用servlet的service()方法 | 这个方法会查看请求，确定HTTTP方法（GET,POST），并在servlet上调用对应的方法，如doGet()，doPost()等 | 不应该覆盖service方法。你的任务是覆盖doGet()和/或doPost()方法，而由 HttpServlet中的service()实现来考虑该调用哪一个方法（ doGet()、doPost()等） |
| doGet()和/或doPost() | service()方法根据请求的HTTP方法（GET,POST）来调用doGet()或doPost()。 | 要是这里写你的业务代码                                       | **至少要覆盖其中之一**                                       |

容器会调servlet的init()方法，但是如果不覆盖init()，就会远行GenerieServlet中的init()。然后到来一个请求时，容器会开始或分配一个线程，调用service()方法，这个方法不用覆盖，所以会远行Httpservlet的service()方法。 HttpServlet service()方法再调用覆盖的doGet()或doPost()。这样一来，每次行我的doGet()或doPost()时，它都在一个单独的方法栈中。

## 5 每个请求都在一个单独的线程中运行

你可能听过别人这么说，“servlet的每个实例……"但这种说法是错误的。任何servlet类都不会有多个实例，只有一种特殊情况例外（称为SingleThreadModel，这其实很槽糕），不过我们还不会讨论这种特殊情况。

**容器运行多个线程来处理对一个servlet的多个请求。**

对应每个客户请求，会生成一对新的请求和响应对象。**准确的说，应该是每一个请求一个线程。容器并不关心是谁的请求**

## 6 servlet的加载和初始化

容器找到servlet类文件时，servlet的生命开始。这基本上都是在容器启动时发生（例如，运行Tomcat时）。容器启动时，它会寻找已经部署的web应用，然后开始搜索servlet类文件（在有关“部署”的一章中，我们将更详细地介绍容器如何寻找servlet）

寻找类只是第一步。

第二步是加载类，这可能在容器启动时发生，也可能在第一个客户使用时进行。你的容器可能允许你来完成类加载，也可能会在它希望的任何时刻加载类。不论你的容器是早就准备好servlet，还是在第一个客户需要时才即时地加载类，在 servlet没有完全初始化之前绝不能运行servlet的service()方法。

**init()总是在第一个service()调用之前完成**

## 7 servlet初始化：对象何时成为一个servlet

servlet从“不存在”状态迁移到“初始化”状态（这意味着已经准备好为客户请求提供服务），首先是从构造函数开始。但是构造函数只是使之成为一个对象，而不是一个servlet.要想成为一个servlet，对象必须具备一些“servlet特性”（servletness）

对象成为一个servlet时，它会得到servlet该有的所有特权，比如能够使用Servletcontext引用从容器得到信息。

为什么我们要关心初始化细节呢？

因为在调用构造函数和init()方法之间，servlet处在一种薛定谔servlet状态。你可能有一些 servlet初始化代码，如得到Web应用配置信息，或查找应用另一部分的个引用，如果在servlet的生命中太早运行这些初始化代码就会失败。不过这很简单，你只要记住一点，不要在servlet的构造函数中放任何东西！

别着急，什么都可以放在init()里。

1. ServletConfig对象

- 每个servlet都有一个ServletConfig对象。
- 用于向servlet传递部署时信息（例如数据库或企业bean的查找名），而你不想把这个信息硬编码写到servlet中（servlet初始化参数）。
- 用于访问ServletContext
- 参数在部署描述文件中配置。

2. ServletContext

- 每个Web应用有一个ServletContext（应该叫做AppContext才对）
- 用于访问web应用参数（也在部署描述文件中配置）
- 相当于一种应用公告栏，可以在这里放置消息（称为属性），应用的其他部分可以访问这些消息（下章还会详细介绍这个内容）.
- 用于得到服务器信息，包括容器名和容器版本，以及所支持AP的版本等

ServletConfig和Servletcontext存在只是为了支持servlet的真正任务：处理客户请求！所以在介绍上下文对象和配置对象如何帮助你完成这个任务之前，先退一步，了解一下请求和响应的基础知识。

你已经知道了，要把一个请求和响应作为参数传递给doGet()或doPost()方法，但是这些请求和响应对象能给你什么呢？这些对象怎么处理，为什么需要这些对象？

## 8 请求和响应

ServletRequest接口（javax.servlet.ServletRequest）

HttpServletRequest接口（javax.servlet.http.HttpServletRequest）**继承了上面的接口**

----------------------------------------------------------------------------------------------------------------

ServletResponse接口（javax.servlet.ServletResponse）

HttpServletResponse接口（javax.servlet.http.HttpServletResponse）**继承了上面的接口**

HttpServletRequest和HttpServletResponse的方法增加了与HTTP协议相关的方法。

## 9 doGet()和doPost()

要记住，客户的请求总是包括一个特定的HTTP方法。如果这个HTTP方法是GET，service()方法就会调用doGet()。如果这个HTTP请求方法是POST，service()方法就会调用doPost()

**你可能不会关心GET和POST以外的其他HTTP方法。**

对，除了GET和POST之外，确实还有其他些HTTP1.1方法，这包括HEAD，TRACEOPTIONS，PUT、 DELETE和CONNECT.

对于这8个方法，除了一个方法外，其余的在HttpServlet类中都有一个匹配的doXXX()方法，所以不仅是doGet()和doPost()，你还可以得到doOptions()， doHead()， doTrace()，doPut()和doDelete()。servlet API中没有处理doConnect()的机制，所以HttpServlet里没有doConnect()

对于大多数（其至可以算是全部）servlet开发来说，你总会使用doGe()（针对简单请求）或doPost()（来接受和处理表单数据），其他的不用考虑。

**GET和POST的区别**

GET的数据参数放在请求行中，POST的请求参数放在请求体中

GET仅仅是简单的获取返回的内容，不会对服务器做任何改变，比如是一些查询参数，用于确定要返回什么。POST则用于发送数据来进行处理，往往是一个更新，可以把它认为是利用POST体的数据来修改服务器上的某些东西。

>**幂等请求**
>
>假如某个用户在点击按钮时后台servlet没有发送明确的响应，这时客户重复点击这个按钮，后台就会重复多次响应。这种请求就叫做幂等请求。
>
>```
>		-------GET 幂等------->
>
>客户									Servlet
>
>		<-------响应 返回-------
>```
>
>**非幂等请求**
>
>```
>		-------POST 非幂等------->
>
>客户											Servlet--------------->[DB]servlet使用POST数据来更新数据库
>
>		<-------响应		返回-------
>```
>
>**我们虽然可以完全在servlet中实现一个非幂等的doGet()方法**，但是GET在HTTP1.1中始终被认为是幂等的。

## 10 请求对象的一些方法

ServletRequest和HttpServletRequest接口提供了大量可以调用的方法，javax.servlet.ServletRequest和javax.servlet.Http.HttpservletRequest.

1. 获取请求行

```java
//Get /day14/demo1?name=zhangsan Http/1.1
String getMethod();//获取请求方式：GET
String getContextPath();//获取虚拟目录：//day14
String getServletPath();//获取Servlet路径：/demo1
String getQueryString();//获取get方式请求参数：name=zhangsan
String getRequestURI();//获取请求URI（统一资源标识符）：/day14/demo1
StringBuffer getRequestURL();//URL（统一资源定位符）：http://localhost/day14/demo1
String getProtocol();//获取协议及版本：HTTP/1.1
String getRemoteAddr();//获取客户机IP地址
```

2. 获取请求头

```java
String getHeader(String name);//通过请求头的名称获取请求头的值
Enumeration<String> getHeaderNames();//过去所有的请求名称
```

3. 获取请求体

只有POST请求方式，才有请求体

步骤

- 获取流对象
- 再从流对象中取出数据

```java
BufferedReader getReader();//获取字符输入流，只能操作字符数组
ServletInputStream getInputStream();//获取字节输入流，可以操作所有类型的数据（文件上传）
```

**获取请求参数通用方式**

```java
String getParameter(String name);//根据参数名称获取参数值
String[] getParameterValues(String name);//根据参数名称获取参数值的数组
Enumeration<String> getParameterNames();//获取所有请求的参数名称
Map<String,String[]> getParameterMap();//获取所有参数的map集合
```

**中文乱码问题**

```java
request.setCharacterEncoding("UTF-8");
```

## 复习：servlet生命周期和API

- 容器要加载类、调用 servlet的无参数构造函数并调用servlet的init()方法，从而初始化servlet
- init()方法（开发人员可以覆盖）在 servlet生中只调用一次，往往在 servlet为客户请求提供服务之前调用。
- init()方法使servlet可以访问ServletConfig和Servletcontext对象，servlet需要从这些对象得到有关servlet配置和Web应用的信息。
- 容器通过调用servlet的destroy()方法来结束servlet的生命。
- servlet一生的大多数时间都是在为某个客户请求对运行service()方法
- **servlet的每个请求都在一个单独的线程中运行，任何特定servlet类都只有一个实例。**
- 你的servlet一般都会扩展javax.servlet.httpHttpservlet，并由此继承service()方法的一个实现，它取一个HttpServletRequest和一个HttpServletResponse作为参数。
- HttpServlet扩展了javax.servlet.GenericServlet，这是一个抽象类，实现了大多数基本 servlet方法
- GenericServlet实现了Servlet接口。
- Servlet相关的类（除了与JSP有关的类）都在以下两个包中：Javax.servlet或javax.servlet.http
- 可以覆盖init()方法，而且必须覆盖一个服务方法（ doGet()、 doPost()等）

## 复习：HTTP和HttpServletRequest

- HttpServlet的doGet()和doPost()方法取一个HttpservletRequest和一个 HttpservletResponse作为参数。
- service()方法根据HTTP请求的HTTP方法（GET,POST等）来确定运行doGet()还是doPost()
- POST请求有一个体；GET请求没有，不过GET请求可以把请求参数追加到请求URL的后面（有时称为“查询串”）.
- GET请求本质上讲（根据HTTP规范）是幂等的它们应当能多次运行而不会对服务器产生任何副作用。GET请求不应修改服务器上的任何东西。但是你也可以写一个非幂等的 doGet（方法（不过这是很糟糕的做法）.
- POST本质上讲不是幂等的，所以要由你来适当地设计和编写代码，如果客户错误地把一个请求发送了两次，你也能正确地加以处理。
- 如果HTML表单没有明确地出“method=POST”，请求就会作为一个GET请求发送，而不是POST请求。如果你的servlet中没有doGet()，这个请求就会失败。
- 可以用getParameter(“paranam”)方法从请求得到参数。返回值总是一个String
- 如果对应一个给定的参数名有多个参数值，要使用getParametervalues(“paranam”)方法来返回一个String数组。
- 从请求对象还可以得到其他东西，包括首部cookie、会话、查询串和输入流

## 响应

下面是一个下载JAR的servlet代码实例

```java
public class CodeReturn extends HttpServlet{
	public void doGet(HttpServletRequest request,HttpServletResponse response) throws IOException,ServletException{
		response.setContentType("application/jar");//因为这是一个jar，而不是HTML
		ServletContext ctx = getServletContext();
		Inputstream is = ctx.getResourceAsStream("/bookCode jar");//表示“名为bookCode.jar”的资源提供一个输入流

		int read=0;
		byte[] bytes=new byte[1024];

		OutputStream os=response.getOutputStream();
		while((read=is.read(bytes))!=-1){
			os.write(bytes,0,read);
		}
		os.close();
	}
}
```

**内容类型**

```
response.setContentType("application/jar");
```

或者说，你至少应该明白这行代码。你必须告诉浏览器你要发回些什么，这样浏览器才能有正确的“举止”：可能启动一个“辅助”应用，如PDF阅读器或视频播放器；也可能向用户呈现HTML，或者把响应的字节保存为一个下载文件等。既然你想知道，可以这么说，谈到内容类型的时候，我们指的就是MIME类型。内容类型是HTTP响应中必须有的一个HTTP首部。

常用MIME类型:

```
text/html
application/pdf
video/quicktime
application/java
Image/jpeg
application/jar
application/octet-stream
application/x-zip
```

所以在日常编码中，我们应该先调用setContentType()，然后再调用获得输出流的方法getWriter()或者getOutputStream()。这样就能保证不会遭遇内容类型和输出流之间的冲突。

**为什么你使用一个servlet发回JAR呢？让Web服务器把它作为一个资源返回不就行了吗？换句话说为什么不让用户点击一个指向JAR的链接，而要让链接指向一个 servlet呢？难道不能把服务器配置为直接返回JAR而不要经过servlet吗？**

配置Web服务器，让用户点击一个HTML链接，指向服务器上的某个资源，比如说JAR文件（就像其他静态资源一样，如JPEG和文本文件），服务器只是把它放在响应中返回。

但是如果在发回输出流之前还想在servlet里做点别的事情。例如，在servlet中实现一些逻辑来确定要发送哪个JAR。或者、也许你要发回的是实时创建的字节。想像一下，如果有这样一个系统，你要从用户得到输入奉数，然后使用这些参数动态地生成一个声音，发送回去。原先是没有这个声音的。换句话说，声音并没有作为一个文件放在服务器上。你要建立这样一个声音，然后把它放在响应中返回。

如果只是发回服务器上的一个JAR.前面这个例子确实有些麻烦，有必要建立一个servlet也许原因很简单，只是要在 servlet中放一些代码。以便在发回JAR的同时，能在数据库中写些有关这个特定用户的信息。也可能必须查看用户是否允许下载这个JAR，为此可能需要先从教据库读取一些信息才能决定。

**输出类型，字符还是字节**

ServletResponse接口提供了两个流可供选择：ServletOutputStream用于输出字节，PrintWrite用于输出字符数据

- PrintWrite

```java
PrintWriter writer=response.getWriter();
writer.println("some text and HTML");
```

- OutputStream

```java
ServletOutputStream out=response.getOutputStream();
out.write(aByteArray);
```

PrintWriter实际上包装了ServletOutputStream。也就是说，前者是后者的一个引用，而且会把调用委托给后者。返回给客户的输出流只有一个。但是PrintWriter会修饰这个流，为它添加更高层的字符处理方法。

**设置或者增加响应首部**

```java
为响应增加个新首部和值，或者向一个现有的首部增加另一个值。
response.setHeader("foo","bar");//如果响应中已经有同名的首部，则用这个值替换原来的值。否则，向响应增加一个新首部和值。
response.addHeader("foo","bar");//如果响应中已经有同名的首部，则用这个值替换原来的值。否则，向响应增加一个新首部和值。
response.setIntHeader("foo",42);//这是一个便利方法，用提供的整数值替换现有首部的值，或者向响应增加一个新首部和值。
```

如果响应中还没有首部（方法的第一个参数），setHeader()和addHeader()就会增加一个首部和相应的值。二者的区别是，如果已经有这样一个首部，前者将设置一个值，而后者将增加个值。在这种情况下：

```java
setHeader();//会覆盖现有的值
addHeader();//会增加另外一个值
```

调用 setContentType("text/html")时，就是在设置一个首部相当于：

```java
setHeader("content-type",text/html)
```

### 重定向和转发（请求分派）

#### 重定向

重定向是让浏览器完成工作，servlet只是调用sendRedirect()方法

```java
if(worksForMe){
	//handle the request
} else{
	response.sendRedirect("http://www.oreilly.com");
}
```

sendRedirect中使用相对URLs

可以使用相对URL作为 sendredirect()的参数，而不是指定完整的“http:www…”。相对URL有两种类型：前面有斜线（“/”）和没有斜线

假设客户原来键入的是：

```
http://www.xxx.com/test/com/login.do
```

请求到达名为"bar.do"的servlet时，这个servlet会基于一个相对URL来调用sendRedirect()，这个相对URL没有用斜线开头：

```
sendRedirect("foo/index.html");
```

容器归相对于原先的请求URL建立完整的URL

```
http://www.xxx.com/test/com/foo/index.html
```

但是如果sendRedirect()的参数确实以一个斜线开头：

```
sendRedirect("/foo/index.html");
```

那么容器就会对于Web应用本身建立完整的URL，而不是相对于请求原来的URL，所以新的URL是：

```
http://www.xxx.com/foo/index.html
```

**不能在写到响应之后再调用sendRedirect()**

### 请求分派

```java
RequestDispatcher view=request.getRequestDispatcher("result.jsp");
view.forward(request,response);
```

请求分派是servlet决定这个请求应该交给Web应用的另一个部分

## 复习：HttpServletResponse

- 使用响应向客户发回数据。
- 对响应对象（HttpServletResponse）调用的最常用的方法是setContentTypeo()和getWriter()
- 要当心—很多开发人员都认为应该是getWriter()，但实际上得到书写器的方法是getWriter()
- 利用getWriter()方法可以完成字符I/O，向流写入HTML（或其他内容）
- 还可以使用响应来设置首部、发送错误，以及增加cookie.
- 在实际中，可能会使用JSP发送大多数HTML响应，但仍有可能使用一个响应流向客户发送二进制数据（如JAR文件）
- 要得到二进制流，需要在响应上调用getOutputStream()
- setContentType()方法告诉浏览器如何处理随响应到来的数据。常见的内容类型为"text/html"."application
  /pdf和"image/jpeg"
- 不用记住内容类型（也称为MIME类型）
- 可以使用addHeader()或setHeader()设置响应首部。二者的区别是这个首部是否已经是响应的一部分。如果是， setHeader()会替换原来的值，而addHeader()会向现有的响应增加另一个值。如果响应中原来没有这个首部， setHeader()和addHeader()的表现完全一样。
- 如果你不想对一个请求做出响应，可以把请求重定向到另一个URL.浏览器会负责把新请求发送到你提供的URL
- 要重定向一个请求，需要在响应上调用sendRedirect(aStringURL)
- 不能在响应已经提交之后才调用sendRedirect()换句话说，如果已经向流中写了东西，再想重定向则为时已晚
- 请求重定向与请求分派完全是两码事。请求分派（下一章将详细介绍）在服务器端发生，而重定向在客户端进行。请求分派把请求传递给服务器上的另一个组件（通常在同个Web应用中）.请求重定向只是告诉浏览器去访问另一个URL

# 3 Web应用

## 1 初始化参数

如果我们想把一些信息配置在DD中，而不是硬编码写到servlet中，那么我们可以使用初始化参数来完成这个功能。

比如在DD文件（web.xml）中：

```xml
<servlet>
	<servlet-name></servlet-name>
	<servlet-class></servlet-class>
	<init-param>
		<param-name>adminEmail</param-name>
		<param-value>xxx@xx.com</param-value>
	</init-param>
</servlet>
```

在servlet代码中：

```
out.println(getServletConfig().getInitParameter("adminEmail"))
```

每个servlet都继承了一个getServletConfig()方法，该方法返回一个ServletConfig。getInitParameter()是ServletConfig的一个方法。

**在servlet初始化之前不能使用servlet初始化参数**

容器初始化一个servlet时，会为这个servlet创建一个唯一的ServletConfig。

容器从DD读出servlet初始化参数，并把这些参数交给ServletConfig。然后把ServletConfig传递给servlet的init()方法。servlet运行服务方法（doGet()或doPost()等）时，他已经有了一个ServletConfig。

**servlet初始化参数只能读一次——就是在容器初始化servlet的时候**一旦参数置于ServletConfig中，就不会再读了，除非你重新部署servlet。

## 2 上下文初始化参数

上下文初始化参数与servlet初始化参数很类似，只不过上下文参数对整个Web应用可用，而不是只针对一个servlet。我们不必操心为每个servlet配置DD，而且如果值有变化，只需要在一个地方修改就行了。

在DD文件中：

```xml
<context-param>
	<param-name>adminEmail</param-name>
	<param-vlaue>xxx@xxx.com</parma-value>
<context-param>
```

上面的param-name和parma-value，指定在context-param中，而不是init-param。

`<context-param>`不需要放在某个`<servlet>`元素中，而是放在`<web-app>`里，但是要放在所有`<servlet>`声明之外。

在servlet代码中：

```
out.println(getServletContext().getInitParameter("adminEmail"));
```

所以每一个servlet有一个ServletConfig，每一个Web应用有一个ServletContext。

**如果应用是分布式应用，那么每个JVM都有一个ServletContext**

和ServletConfig一样，ServletContext也只会在重新部署Web应用后，才可以看到修改的XML来改变一个初始化参数的值对servlet和Wen应用其他部分的改变。

现在思考这样一个问题，如果希望应初始化参数是一个数据库DataSource呢？

上下文参数只能是String.毕竟，你不能把一个Dog对象硬塞到XML部署描述文件中（实际上，确实可以用XML表示一个串行化对象，但是在当前的Servlet规范中还没有相关的支持……没准将来会提供）。

如果你真的想让web应用的所有部分都能访问一个共享的数据连接，该怎么做呢？当然可以把这个DataSource查找名放在一个上下文初始化参数里，这也是当前上下文参数最常见的一种用法

不过，在此之后谁将这个String参数转换成由Web应用各部分共享的一个具体DataSource引用呢？

不能把这个代码放在servlet中，因为你选择谁作为第一个servlet来查找DataSource，并把它存储在一个属性中呢？你真的想保证总是让某个特定的Servlet最先运行吗？好好考虑一下。

我们需要一个监听者，能监听一个上下文初始化事件，就能得到上下文初始化参数，并在应用为客户提供服务之前运行一些代码。在普通的Java应用中我们有main()方法，可以实现这个任务，但是对于servlet，我们应该怎么办呢？

### 4 上下文监听者

ServeltContestListener可以监听ServletContext一生中的两个关键事件，初始化和撤销。这个类实现了javax.servlet.ServletContextListener

我们需要一个单独的对象，它能做到：

- 上下文初始化（应用正在得到部署）时得到通知。

  - 从Servletcontext得到上下文初始化参数。
  - 使用初始化参数査找名建立一个数据库连接。
  - 把数据库连接存储为一个属性，使得Web应用的各个部分都能访问
- 上下文撤销（应用取消部署或结束）时得到通知。
- 关闭数据库连接。

ServletContextListener类

```java
import javax.servlet.*

public class My Servlet ContextListener implements ServletcontextListener {
	public void contextInitialized(ServletContextEvent event) {
		//code to initialize the database connection
		//and store it as a context attribute
	}
	public void contextDestroyed(ServletContextEvent event) {
		//code to close the database connection
	}
}
```

#### 实例

在这个例子中，我们要把String初始化参数转换为一个真正的对象——一个Dog。监听者的任务是得到有关狗品种（Beagle，Poodle等）的上下文初始化参数，然后使用这个String。来构造一个Dog对象。监听者再把这个Dog对象存放到一个Servletcontext属性中，以便 servlet获取。

关键是， servlet现在能访问一个共享的应用对象（这里就是一个共享的Dog），而且不必读取上下文参数。这个共享的对象是一个Dog还是一个数据库连接并没有关系。重点是使用初始化参数来创建一个对象，让应用的所有部分都能共享这个对象。

Dog例子：

- 监听者对象向ServletContextevent对象请求应用Servletcontext对象的一个引用。
- 监听者使用这个Servletcontext引用得到“breed”的上下文初始化参数，这是一个String，表示狗的品种。
- 监听者使用这个狗品种String构造一个Dog对象。
- 监听者使用Servletcontext引用在ServletContext中设置Dog属性。
- 这个web应用的测试servlet从ServletContext得到Dog对象，并调用这个Dog的getBreed()方法。

**建立和使用一个上下文监听者**

你可能还在考虑，容器怎样发现和使用监听者，就像告诉容器web应用其他部分的有关情况一样，要用同样的办法配置监听者，对，就是通过web.xml部署描述文件！

1. 创建一个监听者类。

```java
interface ServletContextListener{
	contextlnitialized(ServletContext Event);
	contextDestroyed(ServletContext Event);
}
```

```java
class MyServletContextListener implements ServletContextListener{
	contextlnitialized(ServletContext Event);
	contextDestroyed(ServletContext Event);
}
```

2. 把类放入WEB-INF/classes中
3. 在web.xml部署描述文件放一个`<listener>`元素（放在`<web-app>`下面）

```xml
<listener>
	<listener-class>
		com.example.MyServletContextListener
	</listener-class>
</listener>
```

**需要3个类和一个部署描述文件**

为了便于测试，我们把所有类都放在同一个包中：com.example

1. ServletcontextListener

MyServletContextListenerjava这个类实现了ServletContextListener，它得到上下文初始化参数，创建Dog，并把Dog设置为上下文属性。

```java
package com.example;
import javax.servlet.*;
public class My ServletContextListener implements ServletContextListener {
	public void contextInitialized(Servletcontext Event event) {
		ServletContext sc=event.getServletContext();//由事件得到ServletContext
		String dogBreed=dc.getInitParameter("breed");//使用上下文得到初始化参数
		Dog d=new Dog(dogBreed);//建立一个新Dog
		sc.setAttribute("dog",d);//使用上下文来设置一个属性（名/对象对），这个属性就是Dog，现在应用的其他部分就能得到这个属性（Dog）的值
	}
	public void contextDestroyed(ServletContext Event event) {
		//nothing to do here
	}
}
```

2. 属性类

Dog.java

Dog类只是一个普通的Java类。它的任务是作为个属性值，由ServletContextlistener实例化，井设置在ServletContext中，以便servlet获取。

```java
pachage com.example;
public class Dog{
	private String breed;
	
	public Dog(String breed){
		this.breed=breed;
	}
	public String getBreed(){
		return breed;//servlet会从上下文得到Dog（就是监听者设置为属性的那个Dog）,调用该Dog的getBreed()方法，并把品种打印到响应中，以便我们在浏览器看到
	}
}
```

3. Servlet

ListenerTester.java

这个类扩展了Httpservlet。它的任务是验证监听者的工作，为此，这个Servlet会从上下文得到Dog属性，然后调用Dog的getBreedo，并把结果打印到响应（以便我们在浏览器上看到）.

```java
package com.example;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.io.*;

public class Listenertester extends Httpservlet{
	public void doGet(HttpServletRequest request,HttpServletResponse response) throws IOException, ServletException{
		response.setcontentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("test context attributes set by listener<br>");
		out.println("<br>");

		Dog dog =(Dog)getServletcontext().getAttribute("dog");//如果监听者正常，第一次调用服务方法之前，Dog就已经放在上下文中了，注意强制类型转换，因为getAttribute()返回的类型是Object，需要对返回结果进行强制类型转换
		out.println("Dogs breed is: " + dog.getBreed();
	}
}
```

4. 编写部署文件

```xml
<web-app>
	<servlet>
	</servlet>
	
	<servlet-mapping>
	</servlet-mapping>
	
	<context-param>
		<param-name>breed</param-name>
		<param-value>Great Dane</param-value>
	</context-param>
	
	<listener>
		<listener-class>
			com.example.MyServletContextListener
		</listener-class>
	</listener>

</web-app>
```

## 5 下面是完整的过程

1. 容器读这个应用的部署描述文件，包括`<listener>`和`<context-param>`元素
2. 容器为这个应用创建一个新的ServletContext。应用的所有部分都会分享这个上下文
3. 容器为每个上下文初始化参数创建一个String名/值对
4. 容器将名/值参数的引用交给ServletContext
5. 容器创建MyServletContextListener类的一个新实例
6. 容器拥用蓝听者的contextinitialized()方法，传入一个新的ServletContextEvent。这个事仵对象有ServletContext的一个引用，所以事件处理代码可以从事件得到上下文，并从上下文得到上下文初始化参数。
7. 监听者向ServletContextEvent请求ServletContext的一个引用
8. 监听者向ServletContext请求上下文初始化参数“breed”
9. 监听者使用初始化参数来构造一个新的Dog对象
10. 监听者把Dog设置为ServletContext中的一个属性
11. 容器建立一个新的Servlet（也就是说，利用初始化参数建立一个新的ServletConfig，为这个ServletConfig提供ServletContext的一个引用，然后调用Servlet的init()方法）。
12. Servlet得到一个请求，并向ServletContext请求属性“dog”
13. Servlet在Dog上调用getBreed()（并将结果打印到HttpResponse）

**需要注意的事，监听者不只是面对上下文事件**

## 6 到底什么是属性？

我们已经了解ServletContext监听者如何创建Dog对象（得到上下文初始化参数之后），以及如何将Dog作为一个属性存储（设置）到 ServletContext，这样应用的其他部分就能得到它了。较早前，在啤酒教程中，我们还看到了servlet如何将对模型调用的结果作为属性保存到请求对象（通常是HttpServletRequest），以便JSP/视图能得利这个值。

属性就是一个对象，可能设置（也称为绑定）到另外3个servlet API对象中的某一个，包括ServletContext，HttpServletRequest（或Servlet-Request）或者HttpSession。可以把它简单地认为是一个映射实例对象中的名/值对（名是一个String，值是一个Object）.在实际中，我们并不知道也不关心它具体如何实现，我们关心的只是属性所在的作用域。换句话说，谁能看到这个属性，以及属性能存活多久。

## 7 属性不是参数

|          | 属性                                   | 参数                                                         |
| -------- | -------------------------------------- | ------------------------------------------------------------ |
| 类型     | 应用/上下文  请求  会话                | 应用/上下文初始化参数  请求参数  Servlet初始化参数（没有会话参数的说法） |
| 设置方法 | setAttribute(String name,Object value) | 不能设置应用和Servlet初始化参数，只能在DD中设置              |
| 返回类型 | Object                                 | String                                                       |
| 获取方法 | getAttribute(String name)              | getInitParameter(String name)                                |

## 8 上下文作用域不是线程安全的

应用中的每一部分都能访问上下文属性，这意味着有多个servlet。多个servlet则说明你可能有多个线程，因为请求是并发处理的，每个请求在一个单独的线程中处理。不论这些请求指向同一个servlet还是不同的servlet，都是如此。

**不仅仅如此，容器还可能为一个servlet启动了多个线程处理多个请求，这也会造成线程不安全**

## 9 同步服务方法是不能保护上下文属性的

假如同步了服务方法，容器就不能为只想servlet A的新请求运行其他的方法，这能防止多个线程运行servlet A的一个服务方法来访问上下文属性，但是这无法拒绝其他servlet的访问，不论其他servlet中的服务方法是否同步。

**不是需要对servlet加锁，而是需要对上下文加锁**

## 10 会话属性的线程安全

当客户打开多个窗口时，就会引发线程的破坏，我们对HttpSession同步来保护会话属性

```java
public void doGet(httPservletrequest request HttpservletresponSe response) throws IOException, ServletException{
	response.setContentType("text/html");
	Printwriter out= response.getwriter();
	out.println("test session attributes<br>");
	Httpsession session = request.getsession();
	synchronized(session) {
		session.setAttribute("foo","22");
		session.setAttribute("bar","42");
		out.println(session.getAttribute("foo")):
		out.println(session.getAttribute("bar"));
	}
}
```

## 11 SingleThreadModel设计用来保护实例变量

以下是servlet规范给关于SingleThreadModel（或STM）接口的说明

```
确保servlet一次只处理一个请求
这个接口没有任何方法。如果一个servlet实现了这个接口，就可以保证不会在该servlet的服务方法中并发执行两个线程。通过同步对servlet单个实例的访问，或者通过维护一个servlet线程池并把每个新请求分派到一个空闲的servlet，servlet容器可以保证这一点。
```

#  5 过滤器和包装器

与servelt非常类似，过滤器就是Java组件，请求发送到servlet之前，可以使用过滤器截获和处理请求，另外servlet结束工作后，但在响应发回给客户之前，可以时候用过滤器处理响应。

容器根据DD中的声明来决定何时调用过滤器。

**过滤器要做的事情**

请求过滤器可以：

- 完成安全检查
- 重新格式化请求首部或体
- 建立请求审计或日志

响应过滤器可以：

- 压缩响应流
- 追加或修改响应流
- 创建一个完全不同的响应

只有一个过滤器接口，Filter，对容器来说，只有一种过滤器，也就是所有实现了Filter接口的组件。

**过滤器是模块化的，而且可以在DD中配置**

## 1 过滤器在3个方面很像servlet

**容器知道过滤器API**

过滤器有自己的API。如果一个Java类实现了Filter接口，对容器来说就有很大不同，这个类会从原先一个普通的Java类摇身一变而成为一个正式的J2EE过滤器。过滤器API的其他成员允许过滤器访问ServletContext，而且可以与其他过滤器链接。

**容器管理过滤器的生命周期**

就像servlet一样，过滤器也有一个生命周期。类似于servlet，过滤器有init()和destroy()方法。对应于servlet的doGet()/doPost()方法，过滤器则有一个doFilter()方法。

**都在DD中声明**

Web应用可以有很多的过滤器，一个给定请求可能导致执行多个过滤器。针对请求要运行哪些过滤器，以及运行的顺序如何，这些都要在DD中声明。

## 2 建立请求跟踪过滤器

```java
package com.example.web;

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
public class BeerRequestFilter implements Filter {
	private FilterConfig fc;
    //必须实现init()，通常只需在其中保存配置（config）对象
	public void init(Filterconfig config) throws ServletException {
		this.fc=config;
	}
    //doFilter中才做具体的工作，需要注意的是这个方法并不取HTTP请求和响应对象做参数，而只是ServeltRequest和ServletResponse对象
	public void doFilter(ServletRequest req,ServletResponse resp,FilterChain chain)
		throws ServletException,IOException {
        
		HttpServletRequest httpReq = (HttpServLetRequest)req;
		String name = httpReq.getRemoteUser();
		
		if(name!=null) {
			fc.getServletContext().log("User "+ name +"" is updating");
		}
		chain.doFilter(req,resp);//接下来要调用的过滤器或者servlet，后面还会更多的介绍这个内容
	}
	
	public void destroy() {
		//完成清理工作
	}
}
```

## 3 过滤器的生命周期

每个过滤器都必须实现Filter接口中的三个方法：init(),doFilter和destroy()

**首先要有一个init()**

容器决定实例化一个过滤器时，就要把握住机会，在init()方法中完成调用过滤器之前的所有初始化任务。前一页显示了最常见的实现；也就是保存FilterConfig对象的一个引用，以备过滤器以后使用

**真正的工作在doFilter()中完成**

每次容器认为应该对当前请求应用过滤器时，就会调用doFilter()方法。 doFilter()方法有3个参数：

- 一个ServletRequest（而不是HttpServletrequest）
- 一个ServletResponse（而不是HttpservletreSponse）
- 一个FilterChain

过滤器的功能要在doFilter()方法中实现。如果过滤器要把用户名记录到一个文件中，就要在doFilter()中完成。你想压缩响应输出吗？也要在doFilter()中实现。

**最后是destroy()**

容器决定删除一个过滤器实例时，会调用destroy()方法，这样你就有机会在真正撤销实例之前完成所需的所有清理工作。

> TIPS
>
> **FilterChain是什么？**
>
> 过滤器设计为一些模块化的“积木”、可以采用多种不同方式把这些过滤器结合起来，共同完成一些事情，要让这一切成为可能， FilterChain功不可没。它知道接下来要发生什么。我们已经提到过，过滤器（更不用说 servlet）并不知道请求所涉及的其他过滤器……但是必須有人知道过滤器执行的顺序才行，这个人就是FilterChain，它由DD中指定的filter元素驱动。
>
> 还要说一句， FilterChain与Filter都在同一个包里即javax.servlet
>
> **在doFilte方法中做了这样一个调用： chain.doFilter()……在 doFilter()调用一个doFilter()做什么？会不会造成无限递归呢？**
>
> FilterChain接口的doFilter()与Filter接口的doFilter()稍有不同。下面来告诉你它们的主要区别：
>
> FilterChain的doFilter()方法要负责明确接下来调用谁的doFilter()方法（如果已经到了链尾，则是确定调用哪个servlet的service()方法）但是Filter接口的doFilter()方法通常只完成过滤，这正是创建过滤器的初衷
>
> 这说明， FilterChain可以调用一个过滤器或一个servlet（取决于是否到达链尾）假设容器能把请求URL映射到一个servlet或JSP，那么链尾总是一个servlet或JSP（当然，这是指JSP生成的servlet（如果容器无法找到所请求的正确資源，就不用过滤器。）

## 4 声明和确定过滤器顺序

在DD中配置过滤器，通常会做3件事

- 声明过滤器
- 将过滤器映射到你想过滤的Web资源
- 组织这些映射，创建过滤器调用序列

声明过滤器

```xml
<filter>
	<filter-name>BeerRequest</filter-name>
	<filter-class>com.example.web.BeerRequestFilt</filter-class>

	<init-param>
		<param-name>LogFileName</param-name>
		<param-value>UserLog.txt</param-value>
	</init-param>
</filter>
```

`<filter>`的规则

- 必须有`<filter-name>`
- 必须有`<filter-class>`
- `<init-param>`是可选的，可以有多个`<init-param>`

声明对应URL模式的过滤器映射

```xml
<filter-mapping>
	<filter-name>BeerRequest</filter-name>
	<url-pattern>*do</url-pattern>
</filter-mapping>
```

声明对应Sevlet名的过滤器映射

```xml
<filter-mapping>
	<filter-name>BeerRequest</filter-name>
	<servlet-name>AdviceServlet</servlet-name>
</filter-mapping>
```

`<filter-mapping>`的规则

- 必须有`<filter-name>`，用于链接到适当的`<filter>`元素。
- `<url-pattern>`或`<servlet-name>`元素这二者中必须有一个
- `<url-pattern>`元素定义了哪些Web应用资源要使用这个过滤器。
- `<servlet-name>`元素定义了哪个Web应用资源要使用这个过滤器

重要提示：关于确定过滤器顺序的容器规则：

当多个过滤器映射到一个给定资源时，容器会使用以下规则：

1）先找到与URL模式匹配的所有过滤器。客户做出一个资源请求时，容器会采用URL映射规则来选择“适当的资源”，但这里不同，因为所有匹配的过滤器都会放在链中！！与URL模式匹配的过滤器会按DD中声明的顺序组成一个链

2）一旦将与URL匹配的所有过滤器都放在链中，容器会用同样的办法确定与DD中`<servlet-name>`匹配的过滤器

**但是这还有一个问题，上面这些可以过滤来自客户的请求，但是还有一些请求是通过转发和请求分派产生的，那要怎么做呢？**

我们可以将过滤器应用到请求分派器，当客户请求来自转发或包含，请求分配和/或错误处理器呢？

为通过请求分派请求的Web资源声明一个过滤器映射

```xml
<filter-mapping>
	<filter-name>MonitorFilter</filter-name>
	<url-pattern>*.do</url-pattern>
	<dispatcher>REQUEST</dispatcher>
		-和/或-
	<dispatcher>INCLUDE</dispatcher>
		-和/或-
	<dispatcher>FORWARD</dispatcher>
		-和/或-
	<dispatcher>ERROR</dispatcher>
</filter-mapping>
```

声明规则

- 必须要有`<filter-name>`
- 必须要有`<url-pattern>`或`<servlet-name>`元素其中之
- 可以有0-4个`<dispatcher>`元素。
- REQUEST值表示对客户请求启用过滤器。
- 如果没有指定`<dispatcher>`元素，则默认为REQUEST.
- INCLUDE值表示对由一个include()调用分派来的请求启用过滤器。
- FORWARD值表示对由一个forward()调用分派来的请求启用过滤器。
- ERROR值表示对错误处理器调用的资源启用过滤器。

## 5 用一个响应端过滤器压缩输出

前面我们展示了一个非常简单的请求过滤器。现在来看一个响应过滤器。响应过滤器稍微难一些，但是它们可能非常有用。利用响应过滤器，可以在servlet完成工作之后，而且是响应发送到客户之前，对响应输出做些处理。所以，不必一开始就介入，不用在servlet拿到请求之前就插手，完全可以在最后再接手，就是servlet得到请求并生成了响应之后。

过滤器在链中总是在servlet之前调用， servlet肯定在链尾。不存在 servlet之后才调用的过滤器。但是…还记得前面的栈图吧。在 servlet完其工作井从（虚拟）栈中弹出后，过滤器还会得到机会执行！

## 6 响应过滤器体系结构

Rachel这样介绍doFilter()方法中代码的基本结构—首先，完成与请求相关的工作，然后调用chain.dofilter()，最后，当servlet（以及链中当前过滤器之后的所有其他过滤器）工作结束，而且控制返回到原先的doFilter方法时，可以对响应再做点工作。

Rachel为压缩过滤器设计的伪代码，真的可以实现我们所述的功能吗？

```java
class MyCompressionFilter implements Filter {
    init();
	public void doFilter(request, response, chain) {
		//请求处理放在这里
		chain.doFilter(request, response);//servlet在这里完成它的工作  ①②
        //这里完成压缩逻辑，既然servlet的工作已经结束了，可以对servlet生成的响应进行压缩了  ③
    }
    destroy();
}
```

1. 过滤器把请求和响应传递给servlet，并且耐心等待机会完成压缩
2. 1. servlet完成它的工作，创建输出，完全不知道这个输出要被压缩
   2. 输出通过容器返回...
   3. 发回给客户，这里出现了问题。过滤器本来希望能在输出交给客户之前对输出做出压缩处理
3. chain.doFilter()调用返回，过滤器希望拿到输出，并开始压缩...不过，为时已晚，容器并没有为过滤器把输出缓存起来。等到过滤器自己的doFilter()方法置于（概念）栈的栈顶时，过滤器再想影响（处理）输出为时已晚。

## 7 实现自己的响应