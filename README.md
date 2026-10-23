# nginx-learning

NGINX(8)  
HTTP 服务器与反向代理服务器、邮件代理服务器，通用的 TCP/UDP 代理服务器。

高性能、高稳定性、丰富的功能、简洁的配置方式和较低的资源消耗

企业版发行版、商业支持与培训由F5公司提供。

Beginner’s Guide
启动和停止
重载配置，配置文件的结构，
提供静态内容、
配置为代理服务器，与 FastCGI 应用程序连接。

主进程和多个工作进程。主进程读取并评估配置，管理工作进程。
工作进程负责实际处理请求。
工作进程的数量在配置文件中定义，可以是固定的，自动调整为 CPU 核心数

nginx 由模块组成，指令分为简单指令和块指令。
简单指令由名称和参数组成，用空格分隔，分号（;）结尾。

块指令不是以分号结尾，而是以用大括号（{ 和 }）包围的一组附加指令结尾。
一个块指令可以在大括号内包含其他指令，则称之为上下文。
主上下文

根据请求，文件将从不同的本地目录提供

多个域名可以共享同一个 IP 地址、同一个端口，将请求分发给不同的 server 配置块。
这种机制称为 基于名称的虚拟主机（name-based virtual hosting）。
让每个网站使用不同的端口
用户需要在 URL 中输入非标准端口，例如 http://blog.example.com:8080。
防火墙需要开放更多端口。
网站部署和外部访问管理更复杂。
不能直接通过同一个标准 HTTP 入口统一管理所有网站。

HTTPS 必须先建立 TLS 连接，而 HTTP 的 Host 请求头是在 TLS 握手完成后才能读取的。
答案是 TLS 的 SNI（Server Name Indication，服务器名称指示） 扩展。

证书


本课程提供了虚拟机镜像，这样的话是否太重了？Docker？

URI 会被添加到 root 指令中指定的路径之后，以构成本地文件系统中请求文件的路径

最常见的格式是 X.509 证书，通常采用 PEM 编码保存。

证书和私钥不是同一个东西

DV（Domain Validation，域名验证）证书

通过 ACME 协议与服务器上的客户端协作完成验证。

HTTP-01：通过 HTTP 验证
DNS-01：通过 DNS 验证
默认有效期	90 天

通配符证书（Wildcard Certificate）必须使用 DNS-01 验证

openssl x509 -in cert.pem -noout -text


Visual Studio Code Remote - SSH 扩展允许你在任何装有运行中的 SSH 服务器的远程机器、虚拟机或容器上打开远程文件夹，并充分利用 VS Code 的功能集。连接到服务器后，你可以与远程文件系统上的任意位置的文件和文件夹进行交互。

要获得这些优势，你的本地机器上无需存放源代码，因为该扩展会直接在远程机器上运行命令和其他扩展。该扩展将在远程操作系统中安装 VS Code Server；该服务器独立于远程操作系统上任何已有的 VS Code 安装。

Remote Development与 Remote-SSH extension

可选：如果您的 Linux 或 macOS SSH 主机将同时被多个用户访问，请考虑在 VS Code 用户设置中启用 Remote.SSH: Remote Server Listen On Socket，以提高安全性。

https://code.visualstudio.com/docs/remote/troubleshooting#_configuring-key-based-authentication
命令面板（F1、Ctrl+Shift+P）选择“Remote-SSH: Connect to Host...”，
see a detailed log in the Remote - SSH output channel.

CTRL `

正则表达式 > 最长前缀的 location

include 

上下文（contexts）的顶级指令，将适用于不同流量类型的指令分组

在每个上下文中，您包含一个或多个 server 块来定义控制请求处理的虚拟服务器。根据流量类型，可以在 server 上下文中包含的指令有所不同。

对于 HTTP 流量（http 上下文），每个 server 指令控制对特定域名或 IP 地址上的资源请求的处理。server 上下文中的一个或多个 location 上下文定义了如何处理特定 URI 集合。

对于邮件和 TCP/UDP 流量（mail 和 stream 上下文），每个 server 指令控制对到达特定 TCP 端口或 UNIX 套接字的流量的处理。

继承覆盖

NGINX Control REST API

负载均衡，是优化资源利用率、最大化吞吐量、减少延迟以及提高容错能力提供第 7 层（应用层）负载均衡。

Single Point of Failure, SPOF

负载均衡。首先，使用 upstream 指令定义服务器组。将该指令放置在 http 上下文中。
要将请求转发到服务器组，需在 proxy_pass 指令中指定该组的名称。

支持五种负载均衡方法：轮询（Round Robin）、最少连接（Least Connections）、最短时间（Least Time）、IP哈希（IP Hash）和通用哈希（Generic Hash）。

除轮询（Round Robin）之外的任何方法时，请将相应的指令（hash、ip_hash、least_conn、least_time 或 random）放在 upstream {} 块中 server 指令列表的上方。

需要暂时从负载均衡轮询中移除，可以使用 down 参数对其进行标记。

hash指令的可选consistent参数

server 指令的 weight 参数用于设置服务器的权重。如果未设置权重，则默认为 1。

服务器慢启动功能

在 Nginx 中，动态模块（Dynamic Module）就是可以在 Nginx 主程序编译完成之后，再单独加载的功能模块，不必为了增加一个模块就重新编译整个 Nginx。


Monitoring
Configuring Logging
故障排除和了解流量在基础设施中流动至关重要。

配置错误日志和访问日志、自定义访问日志格式、使用条件日志记录减少冗余信息，以及将日志转发到 syslog 以实现集中收集。
error_log 指令用于设置日志位置和严重级别。日志位置可以是一个特定的文件、stderr 或 syslog。

错误严重级别遵循 syslog 分类系统。

错误日志的默认设置在全局生效。若要覆盖该设置，需将 error_log 指令置于主（顶级）配置上下文中。
每条错误消息仅会被写入到离错误发生层级最近的错误日志中。但是，如果在同一层级指定了多个 error_log 指令，则消息会被写入所有指定的日志中。

请求处理完毕后，会立即将其记录在访问日志中。access_log 指令用于指定日志的位置及其格式。

消息格式为预定义的 combined 格式。若要更改日志消息的格式，请使用 log_format 指令。日志格式通过变量来定义。

当网站访问很慢时，究竟是 Nginx 自己处理得慢，还是它代理的后端应用处理得慢？

- 客户端与 Nginx 之间的网络较慢。
- Nginx 与 Tomcat 建立连接较慢。
- Tomcat 处理请求较慢。
- Tomcat 返回数据较慢，或者响应正文很大。
- Nginx 还需要把响应传输给客户端。
通过访问日志记录不同阶段的耗时，从而用数据定位问题，而不是靠猜测。
时间变量就可能包含多个值。
$upstream_connect_time	建立上游连接所花的时间	网络连接、TCP 握手、上游连接建立
$upstream_header_time	建立上游连接到收到完整响应头的时间	后端响应是否迟缓、应用处理是否耗时
$upstream_response_time	建立上游连接到接收完响应正文的时间	后端整体响应、响应正文传输
$request_time	Nginx 处理整个客户端请求的时间	用户请求整体耗时

以秒为单位，并具有毫秒级分辨率

buffer：先把日志积累在内存里
access_log /var/log/nginx/access.log upstream_time buffer=32k;
对于带缓冲的访问日志，flush 参数可以控制定时刷新，例如：
access_log /var/log/nginx/access.log upstream_time buffer=32k flush=5s;

open_log_file_cache 是一个用于缓存日志文件描述符的高级性能优化指令。Nginx 每次记录访问日志或错误日志时，都会执行一系列昂贵的文件系统操作，在内存中缓存已经打开的日志文件的文件描述符（File Descriptor）。不仅能提升性能，还特别适用于日志路径中包含变量的情况（例如 access_log logs/$host/access.log;），避免了为每个虚拟主机或每个用户动态打开关闭文件的开销。

条件日志记录允许从访问日志中排除琐碎或不重要的日志条目。在 NGINX 中，条件日志记录是通过在 access_log 指令中使用 if 参数来启用的。
Conditional logging 

http {
    map $status $loggable {
        ~^[23]  0;
        default 1;
    }

    server {
        listen 80;
        server_name example.com;

        access_log /var/log/nginx/access.log combined if=$loggable;
    }
}

Logging to syslog

certbot certificates


ssh-keygen -t ed25519 
ssh-keygen -y -f ~/.ssh/id_ed25519

name -i 
