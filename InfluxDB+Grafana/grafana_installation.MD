# [Installation](http://docs.grafana.org/installation/)

Grafana很容易通过Debian/Ubuntu package (.deb), Redhat/Centos package (.rpm)或手动tarball相关的文件或库。
如果你找不到基于平台的包，你可以从源文件构建

### [Installing on Debian / Ubuntu](http://docs.grafana.org/installation/debian/)
### [Installing on RPM-based Linux (CentOS, Fedora, OpenSuse, RedHat)](http://docs.grafana.org/installation/rpm/)

* 下载
[grafana-2.6.0-1.x86_64.rpm](https://grafanarel.s3.amazonaws.com/builds/grafana-2.6.0-1.x86_64.rpm)

* rpm包的安装

```
$ sudo yum install https://grafanarel.s3.amazonaws.com/builds/grafana-2.6.0-1.x86_64.rpm
```
或者
```
-- CentOS/Fedora/Redhat
$ sudo yum install initscripts fontconfig
$ sudo rpm -Uvh grafana-2.6.0-1.x86_64.rpm

-- OpenSuse
$ sudo rpm -i --nodeps grafana-2.6.0-1.x86_64.rpm
```
* YUM仓库安装

```
$ vi /etc/yum.repos.d/grafana.repo
[grafana]
name=grafana
baseurl=https://packagecloud.io/grafana/stable/el/6/$basearch
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packagecloud.io/gpg.key https://grafanarel.s3.amazonaws.com/RPM-GPG-KEY-grafana
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.cr
$ sudo yum install grafana
```
* RPM GPG Key - [pubic GPG key](sudo yum install grafana)
* 包详细信息
  - 二进制文件 /usr/sbin/grafana-server
  - init.d脚本 /etc/init.d/grafana-server
  - 默认文件（环境变量）/etc/sysconfig/grafana-server
  - 配置文件 /etc/grafana/grafana.ini
  - sytemd服务（systemd可用情况下）grafana-server.service
  - 默认配置日志文件 /var/log/grafana/grafana.log
  - 默认配置的sqlite3数据 /var/lib/grafana/grafana.db

* 开启服务(init.d service)

```
$ sudo service grafana-server start
```
使用grafana用户（安装时创建）开启grafana-server进程。
默认HTTP端口3000， 默认用户组admin。

配置Grafana开机启动：
```
$ sudo /sbin/chkconfig --add grafana-server
```

* 开启服务(通过systemd)

```
$ systemctl daemon-reload
$ systemctl start grafana-server
$ systemctl status grafana-server

$ sudo systemctl enable grafana-server.service
```
### [Installing on Mac OS X](http://docs.grafana.org/installation/mac/)
### [Installing on Windowsl](http://docs.grafana.org/installation/windows/)
### [Installing on Docker](http://docs.grafana.org/installation/docker/)

* 官方docker镜像安装
```
$ docker run -i -p 3000:3000 grafana/grafana
```

* Docker卷和ENV配置

Docker容器暴露两个卷-关于sqlite3数据的目录 - /var/lib/grafana和配置文件/etc/grafana。
当开启容器的时候，你可通过map这些volumes到主机目录。

```
$ docker run -d -p 3000:3000 \
    -v /var/lib/grafana:/var/lib/grafana \
    -e "GF_SECURITY_ADMIN_PASSWORD=secret" \
    grafana/grafana:develop
```

### [Installing using Provisioning (Chef, Puppet, Salt, Ansible, etc)](http://docs.grafana.org/installation/provisioning/)
### [Nightly Builds](http://grafana.org/download/builds.html)
### [Building from source](http://docs.grafana.org/project/building_from_source/)

本指南将帮助您从源代码创建包，并开启grafana，在开发环境中运行。 
Grafana附带了自己所需的后端服务器(完全开源)。
它是Go语言编写的，并具有完整的HTTP API。

* 依赖
  - [Go 1.5]()
  - [NodeJS]()
* 获取代码
```
export GOPATH=`pwd`
go get github.com/grafana/grafana
```
* 构建后端应用
```
cd $GOPATH/src/github.com/grafana/grafana
go run build.go setup              # (only needed once to install godep)
$GOPATH/bin/godep restore          # (will pull down all golang lib dependencies in your current GOPATH)
go run build.go build              # (or 'go build .')
```
 - Windows下需要安装GCC(Sqlite3需要)
* 构建前端应用
  - node(v0.12.0), npm(v2.5.0)和grunt(v0.4.5)
```
npm install
npm install -g grunt-cli
grunt
```
* 后端重编译 - 可用[bra](https://github.com/Unknwon/bra)
* 本来运行Grafana
```
./bin/grafana-server
```
  - 浏览器通过管理员登录(默认 admin/admin) 登录 http://localhost:3000
* Grafana开发

### [Configuration](http://docs.grafana.org/installation/configuration/)

Grafana后端有许多可在.ini配置文件中指定或使用环境变量指定的配置选项。

* 配置文件路径
  - 默认配置文件 $WORKING_DIR/conf/defaults.ini
  - 自定义配置文件 $WORKING_DIR/conf/custom.ini
  - 可用--config参数覆盖自定义配置文件路径
* 使用环境变量
  - 配置文件（如下所示）中的所有选项都可以使用使用语法环境变量覆盖：
  ```
  GF_<SectionName>_<KeyName>
  ```
  -  其中包括用括号的部分名称，所有环境变量都使用大写- 使用_替换.
  ```
  [security]
  admin_user = admin
  
  [auth.google]
  client_secret = 0ldS3cretKey
  
  -- you can override them using:
  export GF_SECURITY_ADMIN_USER=true
  export GF_AUTH_GOOGLE_CLIENT_SECRET=newS3cretKey
  ```
* [paths]
  - data: Grafana用来存放sqlite3数据，基于会话的文件和其他数据的路径。通常通过init.d或systemd命令行指定
  - logs: Grafana用来存放日志的路径。通常通过init.d或systemd命令行指定，可被配置文件或者环境变量可覆盖
* [server]
  - http_addr: 绑定的IP地址。如果为空则绑定所有interfaces
  - http_port: 绑定的端口，默认3000。
    - 如果要使用80，需要给予grafana二进制权限
    ```
    $ sudo setcap 'cap_net_bind_service=+ep' /opt/grafana/current/grafana
    ```
    - 或者重转向80端口
    ```
    $ sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 3000
    ```
    - 另一方是将Nginx或Apache服务器放在Grafana前面作为代理请求
  - protocol: http或https
  - domain: 作为root_url的一部分
  - enforce_domain: 如果主机头不匹配域，此参数能重定向到正确的域名。防止DNS重新绑定攻击。默认为false。
  - root_url: 用于从Web浏览器访问Grafana完整的URL。对于使用Google或者Github OAuth认证非常重要。（回调正确的URL）
  - static_root_path
  - cert_file
  - cert_key
* [database]: Grafana需要一个数据库来存储用户和仪表板（和其他东西）。默认情况下它被配置为使用的是嵌入式数据库sqlite3
  - type: mysql, postgres或sqlite3
  - path: 仅适用于sqlite3，数据库文件存放的路径
  - host: 仅适用于MySQL或者Postgres。包含IP或主机名和端口。例如:host=127.0.0.1:3306
  - name: Grafana数据库名字
  - user: 数据库用户名（不适用sqlite3）
  - password: 数据库用户密码（不适用sqlite3）
  - ssl_mode: 仅用于postgres，可以是disable, require或者verify-full
* [security]
  - admin_user
  - admin_password
  - login_remeber_days
  - secret_key
  - disable_gravatar: 设置true可禁用用户profile图片的唯一标志符，默认为false
* [users]
  - allow_sign_up
  - allow_org_create
  - auto_assign_org
  - auto_assign_org_role
* [auth.anonymous]
  - enabled
  - org_name
  - org_role
* [auth.github]
* [auth.google]
* [auth.basic]
  - enabled: 如果是true(默认)则http api接受基本的认证
* [auth.ldap]
  - enabled
  - config_file
* [auth.proxy]
  - enabled
  - header_name
  - header_property
  - auto_sign_up
* [session]
  - provider: 可以是memory, file(默认), mysql, postgres
  - provide_config: 此选项不同，具体取决于您所配置会话提供的是什么provider。
    - file: 会话文件路径, 例如 data/sessions
    - mysql: go-sql-driver/mysql dsn配置字符串，例如user:password@tcp(127.0.0.1:3306)/database_name
    - postgres: 例如user=a password=b host=localhost port=5432 dbname=c sslmode=disable
    - 如果使用MySQL或Postgre作为会话存储，你需要手动创建会话表。例如MySQL
    ```
    CREATE TABLE `session` (
      `key`       CHAR(16) NOT NULL,
      `data`      BLOB,
      `expiry`    INT(11) UNSIGNED NOT NULL,
      PRIMARY KEY (`key`)
    ) ENGINE=MyISAM DEFAULT CHARSET=utf8;
    ```
  - cookie_name
  - cookie_secure
  - session_life_time
* [analytics]
  - reporting_enabled
  - google_analytics_ua_id
* [dashboards.json]: 如果你使用自动生成的仪表板为JSON文件的系统，您可以启用此功能有Grafana后端指数的JSON仪表板，这将使他们出现在常规仪表盘的搜索。
  - enabled
  - path


```
```





