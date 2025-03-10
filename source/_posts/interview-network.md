---
title: 前端面试题准备 - 网络 & 浏览器
date: 2025-02-09 22:20:17
tags:
  - Interview
  - Frontend
  - Network
categories:
  - Interview
top_img: /img/interview.jpg
cover: /img/interview.jpg
---

## OSI 七层与 TCP/IP 五层模型

**OSI 七层模型：**

- 应用层
- 表示层
- 会话层
- 传输层
- 网络层
- 数据链路层
- 物理层

**TCP/IP 五层模型：**

- 应用层：TFTP，HTTP，SNMP，FTP，SMTP，DNS，Telnet
- 传输层：TCP，UDP
- 网络层：IP，ICMP，RIP，OSPF，BGP，IGMP
- 数据链路层：SLIP，CSLIP，PPP，ARP，RARP，MTU
- 物理层

---

## 应用层的协议哪些是基于 TCP 协议的，哪些是基于 UDP 协议的？

**基于 TCP 协议的协议：**

- **FTP（文件传输协议）**：定义了文件传输协议，使用 `21` 端口。
- **TELNET（远程登陆协议）**：一种用于远程登陆的端口，使用 `23` 端口，用户可以以自己的身份远程连接到计算机上，可提供基于 DOS 模式下的通信服务。
- **SMTP（简单邮件传输协议）**：邮件传送协议，用于发送邮件。服务器开放的是 `25` 号端口。
- **POP3（邮件读取协议）**：它是和 `SMTP` 对应，`POP3` 用于接收邮件。`POP3` 协议所用的是 `110` 端口。
- **HTTP（超文本传输协议）**：是从 Web 服务器传输超文本到本地浏览器的传送协议。
- **HTTPS（超文本传输安全协议）**：是以安全为目标的 `HTTP` 通道，简单讲是 `HTTP` 的安全版。即 `HTTP` 下加入 `SSL` 层，`HTTPS` 的安全基础是 `SSL`，因此加密的详细内容就需要 `SSL`。

**基于 UDP 协议的协议：**

- **TFTP（简单文件传输协议）**：该协议在熟知端口 `69` 上使用 `UDP` 服务。
- **DHCP（动态主机配置协议）**：是一种局域网的网络协议。主要有两个用途：给内部网络或网络服务供应商自动分配 `IP` 地址，给用户或者内部网络管理员作为对所有计算机作中央管理的手段。
- **RIP（路由信息协议）**：基于距离矢量算法的路由协议，利用跳数来作为计量标准。
- **SNMP（简单网络管理协议）**：使用 `UDP` 的 `161` 端口。用于管理网络设备。
- **BOOTP（引导程序协议）**：是 `TCP/IP` 协议族中的一个，用于网络引导，应用于无盘设备。
- **IGMP（Internet 组管理协议）**：是因特网协议家族中的一个组播协议。该协议运行在主机和组播路由器之间。

**基于 TCP 和 UDP 协议的协议：**

- **DNS（域名系统）**：大多数 DNS 查询使用 `UDP` 进行，但当查询超过 `512` 字节时，会使用 `TCP` 进行区域传输。使用 `53` 端口。
- **ECHO（回声协议）**：使用 `TCP` 能可靠地传输数据，使用 `UDP` 能快速地传输数据。使用 `7` 端口。

---

## TCP 和 UDP 的区别

TCP（传输控制协议）和 UDP（用户数据报协议）都是 **传输层协议**。

**TCP** 是 **面向连接、可靠** 的传输协议，适用于需要数据完整性和顺序的场景，比如网页浏览、文件传输等。

- **面向连接**：在通信前需要建立三次握手，确保连接可靠。
- **可靠传输**：数据丢失时，`TCP` 自动重传，保证数据完整性。
- **有序传输**：接收端会按照顺序重组数据，确保数据包不乱序。
- **流量控制**：通过滑动窗口调整发送速率，防止网络拥塞。
- **拥塞控制**：`TCP` 使用算法（如慢启动、拥塞避免）来防止网络过载。

**UDP** 是 **无连接、不可靠** 的协议，适用于对速度要求高、但对数据完整性要求不高的场景，比如视频通话、实时游戏等。

- **无连接**：发送数据前不需要建立连接，发送数据后不需要释放连接。
- **不可靠传输**：不保证数据包的顺序，不保证数据包的完整性，不保证数据包的到达。
- **轻量级**：`UDP` 头部信息较少，开销较小（`UDP` 头部只有 8 个字节，而 `TCP` 头部至少 20 个字节）。
- **快速传输**：没有握手和重传机制，适合低延迟通信。

**TCP 和 UDP 的区别**

- `TCP` 是面向连接的，`UDP` 是无连接的。
- `TCP` 是可靠的，`UDP` 是不可靠的。
- `TCP` 是面向字节流的，`UDP` 是面向报文的。
- `TCP` 只能是一对一，`UDP` 可以是一对一、一对多、多对多。
- `TCP` 首部较大，`UDP` 首部较小。
- `TCP` 有拥塞控制和流量控制，`UDP` 没有。

---

## HTTP 状态码

HTTP 状态码由三位数字组成，第一个数字定义了状态码的类型，后两位数字没有分类作用。

- `1xx`：信息性状态码，表示请求正在处理。
  - `100`：继续
  - `101`：切换协议
- `2xx`：成功状态码，表示请求成功。
  - `200`：请求成功
  - `201`：请求成功并且创建了新的资源
  - `202`：请求已接受，但尚未处理
  - `204`：请求成功，但没有任何内容返回
  - `206`：请求成功，但只有部分内容返回
- `3xx`：重定向状态码，表示需要进一步操作。
  - `301`：永久重定向，表示请求的资源已经被分配了新的 URL，比如启用了新域名、服务器切换到了新机房、网站目录层次重构，这些都算是“永久性”的改变。响应的 Location 首部中应该包含 资源现在所处的 URL
  - `302`：临时重定向，表示请求的资源的 URL 已临时定位到其他位置，客户端应该使用 Location 首部给出的 URL 来临时定位资源。将来的请求仍应使用老的 URL
  - `304`：资源未修改，表示客户端可以继续使用本地缓存
- `4xx`：客户端错误状态码，表示请求错误。
  - `400`：请求错误
  - `401`：未授权，表示客户端没有权限访问该资源，比如没有登录
  - `403`：禁止访问，表示客户端虽然携带了认证信息，但是没有权限访问该资源
  - `404`：未找到资源
  - `405`：方法不允许
  - `429`：请求次数过多
- `5xx`：服务器错误状态码，表示服务器错误。
  - `500`：服务器内部错误
  - `501`：未实现
  - `502`：网关或代理服务器收到无效响应
  - `503`：服务不可用
  - `504`：网关超时

---

## HTTP1.0 和 HTTP1.1 和 HTTP2.0 的区别

**HTTP1.0 和 HTTP1.1 的区别**

1. **缓存处理**：

- `1.0` 的 header 中主要是通过 `If-Modified-Since`（比较资源的最后的更新时间是否一致），`expires`(资源的过期时间，取决于客户端本地时间)
- `1.1` 引入了其他的 `If-Match(比较 ETag 是否一致)`, `If-None-Match(比较 ETag 是否不一致)`, `If-Unmodified-Since(比较资源最后的更新时间是否不一致)`, `Entity tag(资源的匹配信息)`

2. **带宽优化**：

- `1.0` 存在一些浪费带宽的现象，例如客户端只需要某个对象的一部分，但是服务器将整个对象返回。
- `1.1` 则在请求头引入了 range 头域，它允许只请求资源的某个部分，即返回码是 `206（Partial Content）`

3. Host 头处理：

- `1.0` 中认为每个服务器都有一个唯一的 `IP` 地址，因此请求的 `url` 中并没有传递主机名（`hostname`）
- 随着虚拟化技术的发展，一台物理机上可以有多个虚拟机，共享同一个 `IP` 地址，`1.1` 中的请求消息和响应消息都支持 `Host`，请求消息中如果没有 `Host` 头域会报告一个错误（`400 Bad Request`）

4. 长连接：

- `http` 是基于 `TCP/IP` 协议的，创建一个 `TCP` 连接是需要经过三次握手的,有一定的开销，如果每次通讯都要重新建立连接的话，对性能有影响。因此最好能维持一个长连接，可以用个长连接来发多个请求。
- `1.0` 中每次需要使用 `keep-alive` 参数来告知服务器端要建立一个长连接
- `1.1` 默认支持长连接，一定程度上弥补了 `HTTP1.0` 每次请求都要创建连接的缺点

5. 新增状态码：

- `1.1` 中新增了 24 个错误状态响应码，如 `409（Conflict）` 表示请求的资源与资源的当前状态发生冲突；`410（Gone）` 表示服务器上的某个资源被永久性的删除

6. 新增请求方式：

- `PUT`，`DELETE`，`OPTIONS` 等

**HTTP2.0 和 HTTP1.X 的区别**

1. **header 压缩**：header 头部带有大量的信息，而且每次使用报头压缩，降低开销，对于相同的 header 数据，不再通过每次请求和响应发送，差量更新 HTTP 头部，既避免了重复 header 的传输，又减小了需要传输的大小

2. **多路复用**：在一个连接里，客户端和浏览器都可以同时发送多个请求或回应，而且不用按照顺序

3. **二进制分帧**：消息由一个或多个帧组成。多个帧之间可以乱序发送

4. **服务端推送**：`HTTP2` 引入服务器推送，允许服务端推送资源给客户端,服务器会顺便把一些客户端需要的资源一起推送到客户端

---

## HTTP 和 HTTPS 的区别

1. HTTP 传输的数据都是未加密的，也就是明文的，HTTPS 协议是由 HTTP 和 SSL 协议构建的可进行加密传输和身份认证的网络协议，比 HTTP 协议的安全性更高。
2. HTTPS 协议需要 CA 证书。
3. 使用不同的链接方式，端口也不同，一般而言，HTTP 协议的端口为 80，HTTPS 的端口为 443。

---

## HTTPS 协议的工作原理

1. 客户使用 HTTPS URL 访问服务器，则要求 web 服务器建立 SSL 链接，客户端向服务器发送的报文包括客户端所支持的 ssl 版本，支持的加密算法以及密钥的长度。
2. web 服务器接收到客户端的请求之后，也在报文中包含 SSL 版本以及加密组件，服务器的加密组件内容时从接收到的客户端加密组件内筛选出来的。
3. 同时，web 服务器会将网站的 CA 证书（证书中包含了公钥），返回给客户端。
4. 客户端通过 CA 证书来验证服务端的身份，公钥是否有效，比如颁发机构，过期时间，并随机生成对称加密的密钥 X 用公钥加密发给服务端。
5. 服务器拿到客户端发过来的加密内容用自己的私钥解密获取到密钥 X。
6. 双方都拿到了密钥 X，SSL 通道建立完成，通过密钥 X 加密信息来进行通信。

总结：HTTPS 协议使用了 **非对称加密（传输对称密钥） + 对称加密（后续数据传输）** 的方式，即利用了非对称加密安全性高的特点，又利用了对称加密速度快，效率高的好处。

## 在浏览器输入 URL 回车之后发生了什么？

1. **URL 解析**

- 地址解析：判断输入是一个合法的 URL 还是一个搜索关键字，并且根据输入进行自动完成、字符编码等操作。
- HSTS：检查 HSTS 列表(HTTP Strict Transport Security)，如果在列表中则强制使用 HTTPS。
- 其他操作：比如安全检查、访问限制。
- 检查缓存：如果浏览器有缓存，则直接返回缓存内容。
  
2. **DNS 查询**

- 浏览器缓存：浏览器会先检查是否在缓存中，没有则调用系统库函数进行查询。
- 操作系统缓存：操作系统也有自己的 DNS 缓存，但在这之前，会向检查域名是否存在本地的 Hosts 文件里，没有则向 DNS 服务器发送查询请求。
- 路由器缓存：路由器也有自己的 DNS 缓存。
- ISP 的 DNS 缓存：ISP DNS 就是在客户端电脑上设置的首选 DNS 服务器，它们在大多数情况下都会有缓存。
- 根域名服务器查询：在前面所有步骤没有缓存的情况下，本地 DNS 服务器会将请求转发到互联网上的根域。

![DNS 查询流程](img/dns.png)

3. **TCP 连接**

TCP/IP 分为四层，分别是应用层、传输层、网络层、链路层。在发送数据时，每层都会对数据进行包装，增加一些信息，比如传输层会添加 TCP 头，网络层会添加 IP 头，链路层会添加 MAC 头。

![TCP/IP 四层模型](img/TCP_IP.png)

4. **服务器处理请求**

- HTTPD：最常见的 HTTPD 有 Linux 上常用的 Apache 和 Nginx，以及 Windows 上的 IIS。它会监听得到的请求，然后开启一个子进程去处理这个请求。
- 处理请求：接受 TCP 报文，解析请求行、请求头、请求体，并根据请求行中的请求方法，决定如何处理请求。
- 重定向：如果请求的资源需要重定向，则返回 `301` 或 `302` 状态码。浏览器会根据状态码重新发送请求。
- URL 重写：如果请求的资源是真实存在的，则会把这个文件返回给浏览器。否则，服务器会按照规则把请求重写到 一个 REST 风格的 URL 上。然后根据动态语言的脚本，来决定调用什么类型的动态文件解释器来处理这个请求。
  
5. **接受响应**

- 查看 header 信息，比如状态码、内容类型、内容长度等。根据状态码决定如何处理。
- 如果响应资源进行了压缩，需要解压。
- 对响应资源进行缓存。
- 根据响应资源的类型去解析响应内容。

6. **渲染页面**

- HTML 解析
- CSS 解析
- 渲染树
- 执行 JS 脚本

## 进程和线程

1. **进程（Process）**

**进程** 是计算机程序的一个执行实例。每个进程都有自己独立的地址空间、数据、堆栈、以及系统资源（如文件描述符等）。进程是 **资源分配的最小单位**，它由操作系统调度和管理。

**进程的特点**：

- **独立性**：进程之间相互独立，它们有各自独立的内存空间，互不干扰。
- **资源拥有者**：每个进程都拥有独立的资源，如内存、文件句柄等。
- **开销大**：创建进程和进行进程间通信（IPC）通常比较复杂和耗费资源。
- **多任务**：操作系统通过多任务调度来运行多个进程，使得计算机可以同时运行多个程序。

**进程的生命周期**：

- **创建**：进程由操作系统创建，通常是通过运行一个程序。
- **执行**：进程在 CPU 上执行，可以进行计算、I/O 操作等。
- **终止**：执行完成或被操作系统终止时，进程会退出并释放资源。

**进程间通信**：

进程间的通信（IPC，Inter-Process Communication）比较复杂，因为每个进程有自己的内存空间，不能直接访问其他进程的数据。常见的进程间通信方式有管道、消息队列、共享内存、套接字等。

2. **线程（Thread）**

**线程** 是 **进程的最小执行单位**。一个进程可以包含多个线程，这些线程共享进程的资源，如内存和文件描述符。线程通常在同一进程的上下文中执行，因此它们之间的通信非常高效。

**线程的特点**：

- **共享资源**：同一个进程中的线程共享进程的内存、堆栈和文件描述符。
- **开销小**：线程的创建和销毁开销相对较小，因为它们共享进程资源。
- **并发执行**：在多核处理器中，多个线程可以同时并行执行，充分利用 CPU 资源。
- **轻量级**：相对于进程，线程的创建和切换更为高效，因此适合用于处理多个任务。

**线程的生命周期**：

- **创建**：线程由进程创建，并开始执行任务。
- **执行**：线程执行特定任务，通常是程序的代码片段。
- **终止**：线程执行完成或被操作系统终止时，它会释放其使用的资源。

**线程之间的通信**：

线程之间的通信比进程间通信简单，因为它们共享内存空间，因此可以直接访问彼此的变量。然而，这种共享内存也带来了线程安全的问题，多个线程同时操作共享资源时可能导致竞态条件、死锁等问题。

**多进程和多线程**：

- 多进程：多个任务同时运行，互相独立。每个进程都有独立的内存空间，进程之间的通信需要通过进程间通信（IPC）机制。
- 多线程：单个任务拆分成不同部分运行。多个线程共享同一个进程的内存空间，线程之间的通信直接通过共享内存进行。

## 浏览器缓存机制

浏览器缓存分为：**强缓存** 和 **协商缓存**

**在浏览器第一次发起请求时**，本地无缓存，向 web 服务器发送请求，服务器起端响应请求，浏览器端缓存。在第一次请求时，服务器会将页面最后修改时间通过 `Last-Modified` 标识由服务器发送给客户端，客户端记录修改时间；服务器还会生成一个 `Etag`，并发送给客户端。

**浏览器在第一次请求发生后，再次发送请求时**：

- 浏览器请求某一资源时，会先获取该资源缓存的 header 信息，然后根据 header 中的 `Cache-Control` 和 `Expires` 来判断是否过期。若没过期则直接从缓存中获取资源信息，包括缓存的 header 的信息，所以此次请求不会与服务器进行通信。这里判断是否过期，则是强缓存相关。
- 如果显示已过期，浏览器会向服务器端发送请求，这个请求会携带第一次请求返回的有关缓存的 header 字段信息，比如客户端会通过 `If-None-Match` 头将先前服务器端发送过来的 `Etag` 发送给服务器，服务会对比这个客户端发过来的 `Etag` 是否与服务器的相同，若相同，就将 `If-None-Match` 的值设为 false，返回状态 304，客户端继续使用本地缓存，不解析服务器端发回来的数据，若不相同就将 `If-None-Match` 的值设为 true，返回状态为 200，客户端重新请求服务器端返回的数据；客户端还会通过 `If-Modified-Since` 头将先前服务器端发过来的最后修改时间戳发送给服务器，服务器端通过这个时间戳判断客户端的页面是否是最新的，如果不是最新的，则返回最新的内容，如果是最新的，则返回 304，客户端继续使用本地缓存。

![浏览器缓存机制](img/browser_cache.webp)

**强缓存**：

强缓存是利用 http 头中的 `Expires` 和 `Cache-Control` 两个字段来控制的，用来表示资源的缓存时间。强缓存中，普通刷新会忽略它，但不会清除它，需要强制刷新。浏览器强制刷新，请求会带上 `Cache-Control:no-cache` 和 `Pragma:no-cache`。

- `Expires`：表示资源过期时间，是一个绝对时间。
- `Cache-Control`：表示资源可以被缓存的最长时间，是一个相对时间。(优先级高于 `Expires`)

**协商缓存**：

协商缓存就是由服务器来确定缓存资源是否可用，所以客户端与服务器端要通过某种标识来进行通信，从而让服务器判断请求资源是否可以缓存访问。普通刷新会启用协商缓存，忽略强缓存。只有在地址栏或收藏夹输入网址、通过链接引用资源等情况下，浏览器才会启用强缓存。

主要涉及到两组 header 字段：`Etag` 和 `If-None-Match`、`Last-Modified` 和 `If-Modified-Since`。

- `Etag`：表示资源唯一标识，是一个字符串，由服务器生成。如果服务器返回 `304` 状态码，response header 中会包含 `Etag` 字段，即使 `Etag` 没有变化。
- `If-None-Match`：表示客户端希望获取的资源的唯一标识，是一个字符串，由客户端生成。
- `Last-Modified`：表示资源最后修改时间，是一个时间戳。如果服务器返回 `304` 状态码，response header 中不会包含 `Last-Modified` 字段。
- `If-Modified-Since`：表示客户端希望获取的资源的最后修改时间，是一个时间戳。

**在已经有 Last-Modified 的情况下，为什么还需要 Etag？**

- 一些文件也许会周期性的更改，但是他的内容并不改变(仅仅改变的修改时间)，这个时候我们并不希望客户端认为这个文件被修改了，而重新GET。
- `Last-Modified` 只能精确到秒级，如果文件在1秒内被修改多次，那么 `Last-Modified` 不能准确反映文件的新鲜度。
- 某些服务器不能精确地得到文件的最后修改时间。

## LocalStorage 、 SessionStorage 和 Cookie 的区别

**共同点**：都是保存在浏览器端，且同源的。

**区别**：`LocalStorage` 和 `SessionStorage` 是 `HTML5` 提供的一种在浏览器端存储数据的技术，而 `Cookie` 是 `HTTP` 协议中的一部分，是服务器为了识别用户身份而存储在浏览器端的数据。

| 特性           | Cookie                | SessionStorage         | LocalStorage           |
|----------------|-----------------------|------------------------|------------------------|
| 存储大小       | 小（约 4KB 每个 cookie） | 较大（约 5MB - 10MB）   | 较大（约 5MB - 10MB）   |
| 存储范围       | 跨标签页、跨会话，随请求发送到服务器 | 仅限当前标签页/会话   | 跨标签页、跨会话（同域） |
| 过期时间       | 可以设置过期时间，也可以是会话 cookie | 浏览器会话结束时清空   | 永久有效，除非被清除    |
| 可用于服务器   | 会随每个 HTTP 请求发送到服务器 | 仅限客户端使用        | 仅限客户端使用         |
| 访问方式       | `document.cookie`     | `sessionStorage`       | `localStorage`         |
| 安全性         | 可以设置 `HttpOnly` 和 `Secure` 标志，防止跨站脚本攻击（XSS） | 仅限当前会话，不易被外部访问 | 同样容易被 XSS 攻击，且无内建安全机制 |

## XSS 攻击

XSS 攻击（Cross-Site Scripting）是一种安全漏洞，攻击者通过在网页中注入恶意脚本，使得这些脚本在浏览器中执行，从而达到攻击目的。XSS 攻击可以分为 **存储型、反射型和基于 DOM 的 XSS 攻击**。

**存储型 XSS 攻击**：

攻击者将恶意脚本直接注入到目标网站的数据库中，存储在服务器上。当其他用户访问该网站的页面时，服务器会返回包含恶意脚本的内容，导致浏览器执行这些恶意脚本。

例子：攻击者在评论区提交一段恶意 JavaScript 脚本，脚本被存储在数据库中，其他用户访问该页面时执行该脚本。

**反射型 XSS 攻击**：

攻击者将恶意脚本作为 URL 参数发送给受害者，服务器会立即返回并执行该脚本。此类型的攻击没有在服务器上永久存储恶意脚本，攻击只是反射到浏览器端。

例子：攻击者构造一个 URL，包含恶意脚本，当受害者点击该链接时，脚本会在其浏览器中执行。

**基于 DOM 的 XSS 攻击**：

攻击者利用网站的客户端脚本漏洞，通过修改 DOM（文档对象模型）来注入恶意代码。这类攻击不依赖于服务器的响应，而是完全通过客户端的 JavaScript 代码触发。

例子：攻击者通过构造特定的 URL，触发浏览器端的 JavaScript 代码，改变页面内容，执行恶意脚本。

**预防 XSS 攻击**：

1. **输入验证与转义**：所有来自用户的输入都应该进行严格的过滤和转义，确保其中不包含恶意的 JavaScript 代码。对于所有可能插入 HTML 的地方，特别是文本输入框、评论框等，必须进行 HTML 转义。

2. **CSP**：CSP（Content Security Policy） 是一种浏览器安全机制，可以帮助检测并减轻某些类型的攻击，包括 XSS。通过 CSP，可以指定允许加载的脚本来源，限制恶意脚本的执行。例如，设置 `script-src 'self'` 只允许加载同源的脚本，防止第三方恶意脚本执行。

3. **HttpOnly & Secure**：在设置 Cookies 时，使用 `HttpOnly` 属性来防止 JavaScript 访问敏感的 Cookie 数据（如会话 Cookie）。使用 `Secure` 属性来确保 Cookies 只在 HTTPS 请求中传输。

## CSRF 攻击

跨站请求伪造 (CSRF, Cross-Site Request Forgery) 是一种常见的 Web 安全漏洞，攻击者通过诱使已认证的用户访问恶意网站，从而执行未经授权的操作。这种攻击利用了用户已经登录并且有有效身份验证的状态。

**CSRF 攻击的原理**：

1. **用户登录**

服务器会为该用户设置身份验证信息（通常是一个 sessionID 或 token），并通过 Cookie 存储这些信息。

2. **用户访问恶意网站**

用户在浏览器中打开一个恶意网站，该网站包含了伪造的请求，例如，向受害网站提交表单、发送 GET 请求等。

3. **发送请求**

恶意网站的脚本（通常是一个图片、表单或 JavaScript 请求）会自动向用户已经登录的网站发送请求（例如，转账、修改账户信息等）。由于用户已经登录，该请求会携带有效的身份验证信息（如 Cookie），因此受害网站会认为这个请求是合法的。

4. **执行请求**

受害网站由于无法区分是用户主动发起的请求还是恶意网站伪造的请求，因此会错误地执行该请求，可能导致用户账户的资金转移、删除数据、更新个人信息等。

**CSRF 攻击的预防**：

1. **使用 CSRF Token**

每个请求都应该包含一个 CSRF Token，它是一个由服务器生成的随机数，每次请求时都会生成一个新的 Token。这个 Token 在 HTML 页面中以隐藏字段的形式嵌入，并在提交请求时与用户的其他数据一起发送。服务器验证请求中携带的 Token 是否正确，如果不匹配，则认为请求是伪造的，拒绝处理。CSRF Token 应该是不可预测的，并且每次会话中都有一个唯一的 Token。

2. **限制请求来源**

在服务器端限制请求的来源，确保请求来自可信来源。

例如：

使用 `SameSite` 属性。它可以限制浏览器在发起跨站请求时是否会携带 Cookies。

验证检查请求中的 `Referer` 或 `Origin` 头，确保请求来源于允许的域名。

3. **双重验证**

对于敏感操作（如转账、修改密码等），实施双重认证。即使攻击者能够伪造请求，也需要用户进行额外的验证，减少攻击成功的可能性。

## FCP 和 LCP

**FCP (First Contentful Paint)** 和 **LCP (Largest Contentful Paint)** 都是 Web 性能指标，用来衡量网页加载速度和用户体验的质量。它们侧重于不同方面的页面加载过程，帮助开发者理解和优化网站的加载性能。

**FCP**：首次内容绘制，它表示页面在加载过程中首次绘制出内容的时间。

**LCP**：最大内容绘制，它表示页面在加载过程中首次绘制出最大内容的时间。

**FCP 优化**

1. **减少关键渲染路径的阻塞**

减少或延迟加载不必要的 CSS 和 JavaScript 文件，避免它们阻塞页面的初步渲染。

2. **优化图像等资源的加载**

对于非首屏内容的图片和资源，可以使用懒加载技术，延迟加载这些资源，直到它们进入可视区域。

3. **优化服务器响应**

使用更快的服务器、优化数据库查询和 API 响应时间。利用缓存机制（例如浏览器缓存、CDN 缓存、HTTP 缓存头等）减少重新加载资源的时间。

**LCP 优化**

1. **关键资源加载**

通过 `<link rel="preload">` 提前加载 LCP 元素（如图片、视频、字体等），确保这些资源能尽早加载并显示在页面上。

2. **图片和视频优化**

尽量避免对非首屏区域的图片进行阻塞加载。为每种显示尺寸和分辨率提供合适的图片尺寸，避免加载过大的图片。对于视频，可以使用 `preload="auto"` 来优化视频内容的加载，或者采用低质量预加载的视频缩略图。

## TCP 三次握手

TCP 三次握手是 TCP 连接建立的过程，用于确保双方都能正常接收和发送数据。

**三次握手的过程**：

1. **第一次握手**

客户端向服务器发送一个 `SYN` 包，并指明客户端的初始序列号 `ISN(Inital Sequence Number)`，进入 `SYN_SENT` 状态，等待服务器确认。

2. **第二次握手**

服务器收到客户端的 `SYN` 包，也发送一个 `SYN` 包，并指明服务器的初始序列号 `ISN`，同时将 `ACK` 设置为客户端的 `ISN + 1`，进入 `SYN_RCVD` 状态。

3. **第三次握手**

客户端收到服务器的 `SYN+ACK` 包，将 `ACK` 设置为服务器的 `ISN + 1`，发送 `ACK` 包给服务器，进入 `ESTABLISHED` 状态。

服务器收到客户端的 `ACK` 包，进入 `ESTABLISHED` 状态。

第三次握手可以携带数据，因为客户端已经处于 `ESTABLISHED` 状态，可以正常发送和接收数据，并且它已经知道服务器的接收、发送能力是正常的。

**为什么需要三次握手**

1. **确保连接的可靠性**

第一次握手：客户端发送网络包，服务端收到了。这样服务端就能得出结论：客户端的发送能力、服务端的接收能力是正常的。

第二次握手：服务端发包，客户端收到了。这样客户端就能得出结论：服务端的接收、发送能力，客户端的接收、发送能力是正常的。不过此时 **服务器并不能确认客户端的接收能力是否正常**。

第三次握手：客户端发包，服务端收到了。这样服务端就能得出结论：客户端的接收、发送能力正常，服务器自己的发送、接收能力也正常。

2. **防止重复建立连接**

TCP 使用序列号来标识每个数据包的顺序。在建立连接时，客户端和服务器都会选择初始序列号，第三次握手的目的是确认双方的序列号，并保证没有重复的连接。

假设只有两次握手，客户端和服务器之间可能存在“过期的”连接请求。例如，客户端可能在很久以前发起了一个连接请求，但由于网络延迟，服务器并没有及时回应。这时如果只进行两次握手，客户端和服务器可能错误地认为之前的连接仍然有效，这会导致数据包被错误地解释，甚至引发混乱。

## TCP 四次挥手

TCP 四次挥手是 TCP 连接关闭的过程，用于确保双方都能正常关闭连接。刚开始双方都处于 `ESTABLISHED` 状态，客户端发送 `FIN` 包，服务器收到后发送 `ACK` 包，然后服务器发送 `FIN` 包，客户端收到后发送 `ACK` 包，连接关闭。

**四次挥手的过程**：

1. **第一次挥手**

客户端发送一个 `FIN` 报文，报文中会指定一个序列号。此时客户端处于 `FIN_WAIT_1` 状态。

2. **第二次挥手**

服务端收到 `FIN` 之后，会发送 `ACK` 报文，且把客户端的序列号值 + 1 作为 `ACK` 报文的序列号值，表明已经收到客户端的报文了，此时服务端处于 `CLOSE_WAIT` 状态，客户端收到后进入 `FIN_WAIT_2` 状态。

3. **第三次挥手**

如果服务端也想断开连接了，和客户端的第一次挥手一样，发给 `FIN` 报文，且指定一个序列号。此时服务端处于 `LAST_ACK` 状态。

4. **第四次挥手**

客户端收到 `FIN` 之后，会发送 `ACK` 报文，且把服务端的序列号值 + 1 作为 `ACK` 报文的序列号值，表明已经收到服务端的报文了，此时客户端处于 `TIME_WAIT` 状态。需要过一阵子以确保服务端收到自己的 `ACK` 报文之后才会进入 `CLOSED` 状态，避免出现服务端没有收到 `ACK` 报文的情况。

服务端收到 `ACK` 之后，进入 `CLOSED` 状态，表示连接已关闭。

## Preflight

Preflight 是指在发出实际请求之前，浏览器向服务器发送的一种预检请求，主要用于跨源资源共享（CORS）中。它的目的是检查目标服务器是否允许跨域请求，确保请求符合目标服务器的安全策略。

**Preflight 的过程**：

1. **预检请求**

浏览器在发出实际请求之前，会先发送一个预检请求，询问目标服务器是否允许跨域请求。这个请求包括：请求方法、请求头等 CORS 相关信息。

2. **服务器响应**

服务器收到预检请求后，会根据请求头中的信息判断是否允许跨域请求。如果允许，服务器会返回一个预检响应，包括：允许的请求方法、请求头等 CORS 相关信息。

3. **实际请求**

浏览器收到预检响应后，会根据响应头中的信息判断是否允许跨域请求。如果允许，浏览器会发出实际请求。

## 浏览器渲染过程

1. **解析 HTML**

浏览器解析 HTML 文档，然后转换成 DOM 节点，最终形成 **DOM 树**。

2. **解析 CSS**

浏览器解析 CSS，生成 **CSSOM（CSS Object Model）树**。

3. **构建 Render Tree**

渲染树（Render Tree）是结合 **DOM 树**和 **CSSOM 树**，剔除不可见元素（如 display: none）。

4. **页面布局**

渲染树构建完毕之后，元素的位置关系以及需要应用的样式就确定了，这时浏览器会计算出所有元素的大小和绝对位置。

5. **页面绘制**

页面布局完成之后，浏览器会将根据处理出来的结果，把每一个页面图层转换为像素，并对所有的媒体文件进行解码。

**优化渲染性能**

1. **减少重绘（Repaint）和重排（Reflow）**

- 避免频繁修改 `style`，使用 `class` 批量修改。
- 使用 `transform` 和 `opacity` 进行动画（不会触发布局）。
- 使用 `requestAnimationFrame()` 进行动画优化。

2. **减少 DOM 复杂度**

- 过深的 DOM 结构会增加计算开销。
- 使用 `DocumentFragment` 批量更新 DOM。

3. **CSS 优化**

- 避免使用 `@import` 影响渲染。
- 使用 `will-change` 让浏览器提前优化关键动画元素。

4. **Lazy Loading（懒加载）**

- 图片使用 `loading="lazy"` 避免一次性加载过多资源。