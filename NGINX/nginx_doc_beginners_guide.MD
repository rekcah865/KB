# (Beginner’s Guide)[http://nginx.org/en/docs/beginners_guide.html]

本指南介绍如何启动和停止nginx的，并重新加载它的配置，解释了配置文件的结构，
并介绍如何设置nginx使其服务静态内容，如何配置Nginx作为代理服务器，以及如何将其与连接FastCGI应用

nginx的有一个主进程和多个工作进程。 
主进程的主要目的是读取和评估配置，并维持工作进程。 
工作进程做请求的实际处理。 
nginx的使用基于事件的模型和操作系统依赖性机制来有效地分配工作进程中的请求。 
工作进程的数量有配置文件中定义并可以固定为给定的配置或自动调整为可用的CPU内核
（详见[worker_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes) ）。

nginx及其模块的工作方式有配置文件中来确定。 
默认情况下，配置文件名为nginx.conf并放置在目录/usr/local/nginx/conf ， /etc/nginx ，或/usr/local/etc/nginx 

* Starting, Stopping, and Reloading Configuration

```
nginx -s signal
```

signal可以是下列之一：

- stop -快速关机
- quit -正常关机
- reload -重装配置文件
- reopen -重新打开日志文件

一旦主处理接收到的信号重新加载配置，它检查新的配置文件的语法正确性并尝试应用在其提供的配置。 
如果成功，主进程开始新的工作流程，将消息发送到老的工作进程，要求它们关闭。 否则，主进程撤回变化继续以旧的配置工作。 
旧的工作进程，在接收到命令关闭，停止接受新的连接，并继续服务于当前请求，直到所有的要求提供服务，随后就的工作进程处理退出。

也可以通过Unix工具比如kill命令发送信号到nginx进程。
默认ngnix.pid存放在/usr/local/nginx/logs或/var/run。
```
kill -s QUIT <pid>
```

* Configuration File’s Structure

nginx模块有配置文件的指令来控制的。指令分为简单指令和块指令。
 一个简单的指令由名称和空格分隔参数，并以分号结束（ ; ）。
 块指令具有相同的结构作为一个简单的指令，但不是分号而是用一组括号括起来的附加说明（{和} ）。
 放置在配置文件中的上下文以外的指令都被认为是在主上下文。 比如events, http
 在#符号之后行的部分被认为是一个注释。
 
* Serving Static Content

一个Web服务器重要的任务提供了文件（如图像或静态HTML页面）。 
您可以根据不同的要求，将文件放不同的本地目录服务，例如： /data/www （其可包含HTML文件）和/data/images （包含图片）。
这将需要编制配置文件和在http块中设置含有有2个location块的server块。

```
server {
    location / {
        root /data/www;
    }

    location /images/ {
        root /data;
    }
}
```

包含/images/的URI(http://localhost/images/example.png)请求将被映射到/data/www目录
同时如http://localhost/some/example.html的请求将被nginx发送给/data/www/some/example.html

要应用新的配置，启动nginx(如果它是尚未启动)或发送reload信号nginx的的主进程，执行：
```
nginx -s reload
```

如有异常可以尝试从log目录(/usr/local/nginx/logs或/var/log/nginx)下的access.log, error.log找出原因

* Setting Up a Simple Proxy Server



