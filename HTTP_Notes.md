# 《图解HTTP》Notes
## 了解Web及网络基础
* HTTP: HyperText Transfer Protocol, 超文本传输协议/超文本转移协议，是完成从客户端到服务器端等一系列运作流程规则的约定。
* 1989年3月HTTP诞生。
* 目前的3项WWW构建技术
    * SGML(Standard Generalized Markup Language)，标准通用标记语言，作为页面的文本标记语言的HTML(HyperText Markup Language);
    * HTTP，文档传输协议；
    * URL(Uniform Resource Locator)，统一资源定位符。
* TCP/IP是互联网相关的各类协议族的总称，按层次分为
    * **应用层**，决定向用户提供服务时通信的活动，预存了各类通用的应用服务，比如FTP(File Transfer Protocol, 文件传输协议)，DNS(Domain Name System, 域名系统)，HTTP；
    * **传输层**，提供处于网络连接中的两台计算机之间的数据传输，有两个协议TCP(Transmission Control Protocol)和UDP(User Data Protocol)；
    * **网络层**（网络互连层），用来处理在网络上流动的**数据包**（数据包是网络传输的最小数据单位），规定传输路线，把数据包传送给对方；
    * **数据链路层**，用来处理连接网络的硬件部分，包括控制操作系统、硬件的设备驱动等物理设施。
    分层之后方便自由改动。
* MAC(Media Access Control Address)，指网卡所属的固定地址。
* ARP(Address Resolution Protocol)，用来解析地址的协议，根据IP地址查到对应的MAC地址。
* TCP三次握手，SYN(Synchronize)-ACK(Acknowledgement)
* DNS提供域名到IP地址之间的解析服务，计算机及可以被赋予IP地址，也可以被赋予主机名和域名，用户向DNS发送域名，会收到反馈的IP地址，再通过IP地址访问服务器。
* URL(Uniform Resource Locator，统一资源定位符)
* URI(Uniform Resource Identifier，统一资源标识符)，诸如ftp:// http:// ldap:// mailto:等，【协议方案名+登录信息+服务器地址+端口号+带层次的文件路径+查询字符串+片段标识符】

## 简单的HTTP协议
* 协议版本 HTTP/1.1
* 状态码 200, 404, 500等
* 状态码的原因短语 OK, Not Found等
* HTTP协议自身不具备保存之前发送过的请求或响应的功能。
* HTTP使用URI定位网络资源，请求URI`GET http://hackr.jp/index.htm HTTP/1.1`
    1. GET用来获取资源
    2. POST方法用来传输实体的主体
    3. PUT用来传输文件
    4. HEAD用来获得报文首部
    5. DELETE用来删除文件
    6. OPTIONS用来查询针对请求URI指定的资源支持的方法
    7. TRACE用来让web服务器端将之前的请求通信返还给客户端
    8. CONNECT要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信
* 持久连接是指任意一端没有明确提出断开连接，就保持TCP连接状态。
* Cookie会根据服务器发送的响应报文内的Set-Cookie的首部字段信息，通知客户端保存Cookie，帮助服务器识别曾经通信过的客户端。

## HTTP报文内的HTTP信息
* CR(Carriage Return)，回车符，16进制0x0d
* LF(Line Feed)，换行符，16进制0x0a

## 返回结果的HTTP状态码
* 1XX Informational 信息性状态码，接受的请求正在处理
* 2XX Success 成功状态码，请求正常处理完毕
    * 200 OK
    * 204 No Content
    * 206 Partial Content
* 3XX Redirection 重定向状态码，需要进行附加操作以完成请求
    * 301 Moved Permanently
    * 302 Found
    * 303 See Other
    * 304 Not Modified 资源找到但未符合条件请求
    * 307 Temporary Redirect 临时重定向
* 4XX Client Error 客户端错误状态码，服务器无法处理请求
    * 400 Bad Request
    * 401 Unauthorized
    * 403 Forbidden
    * 404 Not Found
* 5XX Server Error 服务器错误状态码，服务器处理请求出错
    * 500 Internal Server Error
    * 503 Service Unavailable

    
## 与HTTP协作的Web服务器
* 一台服务器使用虚拟主机功能可以为多位客户服务。
* 代理，是一种有转发功能的应用程序，代理不改变请求URI，直接发送给目标服务器。
    * 缓存代理，代理转发响应时会预先将资源的副本保存在代理服务器上。
    * 透明代理，转发请求或者响应时不对报文做任何加工。
* 网关，是转发其他服务器通信数据的服务器，和代理很相似，但是可以提供非HTTP协议服务，可以提高通信的安全性，在客户端和网关之间的通信线路上加密处理。
* 隧道，是在相隔很远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序。

## HTTP首部
* HTTP协议的请求和响应报文中必定包含HTTP首部，内容为客户端和服务器分别处理请求和响应提供所需要的信息。
* HTTP请求报文，包括方法、URI、HTTP版本、HTTP首部字段等部分。


```
GET / HTTP/1.1
Host: hackr.jp
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:13.0) ...
Accept: text/html,application/xhtml+xml,application/xml ...
Accept-Language: ja,en-us;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Connection: keep-alive
If-Modified-Since: Fri, 31 Aug 2007 02:02:20 GMT
If-None-Match: "45bae1-16a-46d776ac"
Cache-Control: max-age=0
```
* HTTP响应报文包括HTTP版本、状态码（数字和原因短语）、HTTP首部字段三部分。


```
HTTP/1.1 304 Not Modified
Data: Thu, 07 Jun 2012 07:21:36 GMT
Server: Apache
Connection: close
Etag: "45bae1-16a-46d776ac"
```
* HTTP首部字段传递了重要信息，在客户端与服务器之间以HTTP协议进行通信的过程中，无论是请求还是响应都会使用首部字段，起到传递额外重要信息的作用。为了给浏览器和服务器提供报文主题大小、所使用的语言、认证信息等内容。
    * 通用首部字段，请求报文和响应报文都会使用的首部
        * Cache-Control 控制缓存的行为
        * Connection 逐跳首部、连接的管理
        * Date 创建报文的日期时间
        * Pragma 报文指令
        * Trailer 报文末段的首部一览
        * Transfer-Encoding 指定报文主体的传输编码方式
        * Upgrade 升级为其他协议
        * Via 代理服务器的相关信息
        * Warning 错误通知
    * 请求首部字段，补充了请求的附加内容、客户端信息、响应内容相关优先级等信息
        * Accept 用户代理可处理的媒体类型
        * Accept-Charset 优先的字符集
        * Accept-Encoding 优先的内容编码
        * Accept-Language 优先的语言（自然语言）
        * Authorization Web认证信息
        * Expect 期待服务器的特性行为
        * From 用户的电子邮箱地址
        * Host 请求资源所在服务器
        * If-Match 比较实体标记（ETag）
        * If-Modified-Since 比较资源的更新时间
        * If-None-Match 比较实体标记（与If-Match相反）
        * If-Range 资源为更新时发送实体Byte的范围请求
        * If-Unmodified-Since 比较资源的更新时间（与If-Modified-Since相反）
        * Max-Forwards 最大传输逐跳数
        * Proxy-Authorization 代理服务器要求客户端的认证信息
        * Range 实体的字节范围请求
        * Referer 对请求中URI的原始获取方
        * TE 传输编码的优先级
        * User-Agent HTTP客户端程序的信息
    * 响应首部字段，补充了相应的附加内容，也会要求客户端附加额外的内容信息
        * Accept-Ranges 是否接受字节范围请求
        * Age 推算资源创建经过时间
        * ETag 资源的匹配信息
        * Location 令客户端重定向至指定URI
        * Proxy-Authenticate 代理服务器对客户端的认证信息
        * Retry-After 对再次发起请求的时机要求
        * Server HTTP服务器的安装信息
        * Vary 代理服务器缓存的管理信息
        * WWW-Authenticate 服务器对客户端的认证信息
    * 实体首部字段，补充了资源内容更新时间等与实体有关的信息
        * Allow 资源可支持的HTTP方法
        * Content-Encoding 实体主体适用的编码方式
        * Content-Language 实体主体的自然语言
        * Content-Length 实体主体的大小（字节）
        * Content-Location 替代对应资源的URI
        * Content-MD5 实体主体的报文摘要
        * Content-Range 实体主体的位置范围
        * Content-Type 实体主体的媒体类型
        * Expires 实体主体过期的日期时间
        * Last-Modified 资源最后修改日期时间

```
Content-Type: text/html
Keep-Alive: timeout=15, max=10
```

## 确保Web安全的HTTPS
* HTTP的缺点：
    * 通信使用明文（不加密），内容可能会被窃听
    * 不验证通信方的身份，有可能遭遇伪装
    * 无法证明报文的完整性，所以有可能已遭篡改
* 通过和SSL(Secure Socket Layer，安全套接层)或TLS(Transport Layer Security，安全层传输协议)的组合使用，加密HTTP的通信内容。
* 报文首部未被加密处理，报文主体的内容会被加密处理。
* Dos(Denial of Service，拒绝服务攻击)攻击
* HTTPS=HTTP+加密+认证+完整性保护
* HTTPS通信步骤
    * 客户端通过发送Client Hello报文开始SSL通信，报文中包含客户端支持的SSL的指定版本、加密组建（Cipher Suite）列表（所使用的加密算法及密钥长度等）
    * 服务器可进行SSL通信时，以Server Hello报文作为应答，包含SSL版本以及加密组建；
    * 之后服务器发送Certificate报文，包含公开密钥证书；
    * 最后服务器发送Server Hello Done报文通知客户端，最初阶段的SSL握手协商部分结束；
    * SSL第一次握手结束之后，客户端以Client Key Exchange报文作为回应，报文中包含通信加密中使用的一种被称为Pre-master secret的随机密码串，该报文已用第三步的公开密钥进行加密；
    * 客户端继续发送Change Cipher Spec报文，提示服务器再次报文之后的通信会采用Pre-master secret密钥加密；
    * 客户端发送Finished报文，包含连接至今全部报文的整体校验值，这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准；
    * 服务器同样发送Change Cipher Spec报文；
    * 服务器同样发送Finished报文；
    * 服务器和客户端的Finished报文交接完毕之后，SSL连接建立完成，通信受到SSL的保护，开始进行应用层协议的通信，发送HTTP请求；
    * 应用层协议通信，发送HTTP响应；
    * 最后由客户端断开连接，发送close_notify报文，之后再发送TCP FIN报文来关闭与TCP的通信。
* HTTPS比HTTP慢2-100倍。

## 确认访问用户身份的认证
* 核对信息
    * 密码
    * 动态令牌
    * 数字证书
    * 生物认证
    * IC卡等
* HTTP使用的认证方式
    * BASIC认证（基本认证）
    * DIGEST认证（摘要认证）
    * SSL客户端认证
    * FormBase认证（基于表单认证）


## 基于HTTP的功能追加协议
* HTTP标准的瓶颈区
    * 一条连接上只能发送一个请求；
    * 请求只能从客户端开始，客户端不可以接受除了响应之外的指令；
    * 请求/响应首部未经压缩就发送，首部信息越多延迟越大；
    * 发送冗长的首部，每次互相发送相同的首部造成的浪费较多；
    * 可任意选择数据压缩格式，非强制压缩发送。


## 构建Web内容的技术
* Web介面几乎全由HTML构建。
* CSS(Cascading Style Sheets，层叠样式表)可以指定如何展现HTML内的各种元素。
* XML(eXtensible Markup Language，可扩展标记语言)是一种可按应用目标进行扩展的通用标记语言。
* JSON(JavaScript Object Notation)是一种以JavaScript的对象表示法为基础的轻量级数据标记语言，能够处理的数据类型有false/null/true/对象/数组/数字/字符串，共7种。


==摘录自《图解HTTP》上野 宣 著（于均良 译）==