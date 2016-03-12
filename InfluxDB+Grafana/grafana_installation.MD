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







