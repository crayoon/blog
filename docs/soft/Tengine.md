## 1、安装 Tengine
在主要核心的组件安装完毕以后就可以安装 Tegine 了，最新版本的 Tegine 可从官网(https://tengine.taobao.org/)获取。
在编译安装前还需要做的一件事是添加一个专门的用户来执行 Tengine。当然你也可以用 root（不建议）。
```
groupadd www
useradd -s /sbin/nologin -g www www
```
接下来才是进行安装：
```
cd /usr/local/src
wget https://tengine.taobao.org/download/tengine-2.2.2.tar.gz
tar -zxvf tengine-2.2.2.tar.gz
cd tengine-2.2.2
./configure --prefix=/usr/local/nginx --user=www --group=www --with-pcre=/usr/local/src/pcre-8.39 --with-openssl=/usr/local/src/openssl-1.1.0c --with-jemalloc=/usr/local/src/jemalloc-4.4.0 --with-http_gzip_static_module --with-http_realip_module --with-http_stub_status_module --with-http_concat_module --with-zlib=/usr/local/src/zlib-1.2.11 --with-ipv6 --with-mail --with-http_ssl_module  --with-http_v2_module
make && make install
```
注意配置的时候 –with-pcre 、–with-openssl、–with-jemalloc、–with-zlib 的路径为源文件的路径。

2、配置 Tengine，设置 tengine 自动启动
系统用户登录系统后启动的服务 的目录
```
/usr/lib/systemd/system
```
如需要开机没有登陆情况下就能运行的程序在系统目录内
/lib/systemd/system
我希望系统开机就启动目录，所以我把文件放在系统目录内。
```
vim /lib/systemd/system/nginx.service

[Unit]
Description=The nginx HTTP and reverse proxy server
After=syslog.target network.target remote-fs.target nss-lookup.target
 
[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
 
[Install]
WantedBy=multi-user.target
```
修改文件权限 chmod 745 nginx.service
设置为开机启动 systemctl enable nginx.service

启动 nginx 服务 systemctl start nginx.service
设置开机自启动 systemctl enable nginx.service
停止开机自启动 systemctl disable nginx.service
查看服务当前状态 systemctl status nginx.service
重新启动服务 systemctl restart nginx.service

作者：centrexzj
链接：https://ld246.com/article/1536585912527