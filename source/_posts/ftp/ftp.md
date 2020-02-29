---
title: ftp服务器搭建
tags:
- ftp
---

# 安装环境

```bash
yum -y install vsftpd lftp
```

<!--more-->

```bash
# 开启
sudo service vsftpd start
# 开机启动
systemctl enable vsftpd.service
# 查看ftp服务
netstat -antup | grep ftp
```

# vsftpd的文件

ftp端口: 20和21

20端口用于数据传输
21端口用于命令传输

默认共享目录: /var/ftp/pub/
配置文件: /etc/vsftpd/vsftpd.conf
指定哪些用户不能访问ftp: /etc/vsftpd/ftpusers
指定哪些用户能访问ftp: /etc/vsftpd/user_list
默认匿名用户的根目录: /var/ftp/

# 配置

+ local_enable:	YES/NO, 是否允许本地系统用户登录
+ write_enable: YES/NO, 是否开启任何形式的 FTP 写入命令, 上传文件
+ local_umask: 本地用户的 umask 设置, 如果注释该设置则默认为 077, 但一般都设置成 022
+ dirmessage_enable: YES/NO, 当进入某个目录时, 发送信息提示给远程用户
+ xferlog_enable: YES/NO, 是否开启 上传/下载 的日志记录
+ connect_from_port_20: YES/NO, 是否使用 20 端口来连接 FTP
+ xferlog_file: 有效路径, 设置日志文件的保存位置, 默认为 /var/log/xferlog
+ xferlog_std_format: YES/NO, 是否使用标准的 ftpd xferlog日志格式, 该格式日志默认保存在 /var/log/xferlog
+ idle_session_timeout: 数值, 设置空闲连接的超时时间, 单位 秒
+ data_connection_timeout: 数值, 设置等待数据传输的最大时间, 单位 秒(data_connection_timeout 与 idle_session_timeout 在同一时间只有一个有效)
+ async_abor_enable: YES/NO, 是否支持异步 ABOR 请求
+ ascii_upload_enable: YES/NO, 是否开启 ASCII 模式进行文件上传, 一般不开启
+ ascii_download_enable: YES/NO, 是否开启 ASCII 模式进行文件下载, 一般不开启
+ chroot_local_user: YES/NO, 是否将所有用户限制在主目录, 当为 NO 时, FTP 用户可以切换到其他目录
+ chroot_list_enable: YES/NO, 是否启用限制用户的名单列表
+ chroot_list_file: 有效文件, 用户列表, 其作用与 chroot_local_user 和 chroot_local_user 的组合有关
+ allow_writeable_chroot: YES/NO, 是否允许用户对 ftp 根目录具有写权限, 如果设置成不允许而目录实际上却具备写权限, 则会报错
+ listen: YES/NO, 如果为 YES, vsftpd 将以独立模式运行并监听 IPv4 的套接字, 处理相关连接请求(该指令不能与 listen_ipv6 一起使用)
+ listen_ipv6: YES/NO, 是否允许监听 IPv6 套接字
+ pam_service_name: 设置 PAM 外挂模块提供的认证服务所使用的配置文件名
+ userlist_enable: YES/NO, 是否启用 user_list 文件来控制用户登录
+ userlist_deny: YES/NO, 是否拒绝 user_list 中的用户登录, 此属性设置需在 userlist_enable = YES 时才有效
+ tcp_wrappers: YES/NO, 是否使用 tcp_wrappers 作为主机访问控制方式
+ max_clients: 数值, 同一时间允许的最大连接数
+ max_per_ip: 数值, 同一个IP客户端连接的最大值
+ local_root: 有效目录, 系统用户登录后的根目录
+ user_config_dir: 有效目录, 用户单独配置文件存放目录, 该目录下用户的文件名就是对应用户名
+ anon_root: 有效目录, 匿名用户登录后的根目录
+ anonymous_enable: YES/NO, 允许匿名访问模式
+ anon_umask=022: 匿名用户上传文件的umask值(总权限值777, 022表示去掉的权限, 最终权限755)
+ anon_upload_enable: YES/NO, 是否允许匿名用户上传文件, 如果要设置为允许, 则需要先开启 write_enable, 否则无效, 此外对应目录还要具有写权限
+ anon_mkdir_write_enable: YES/NO, 允许匿名用户创建目录
+ anon_other_write_enable: YES/NO, 允许匿名用户修改目录名称或删除目录
+ guest_enable: YES/NO, 开启虚拟用户支持
+ guest_username: 虚拟用户名

# 配置账户登录访问

建立一个用户, 该用户能访问ftp, 但不能登录本地系统
```bash
useradd -s /sbin/nologin ftpadm
echo ftp123 | passwd --stdin ftpadm
```

```bash
local_enable=YES
write_enable=YES
local_umask=022
local_root=/var/ftp/pub
anonymous_enable=NO
anon_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
allow_writeable_chroot=YES
pam_service_name=vsftpd
port_enable=YES
```

需要修改文件: /etc/pam.d/vsftpd

注释该语句
```
#auth       required    pam_shells.so
```

# ftp虚拟用户

1. 创建用户文本文件, 在此文件中添加虚拟用户, 两行为一组分别对应用户名和密码

vim /etc/vsftpd/vusers.list

```txt
test
123
boss
123
```

2. 建立用户数据库

按装
```bash
yum -y install db4-utils
```

生成用户数据库

```bash
db_load -T -t hash -f vusers.list vusers.db
```

+ -T: 允许应用程序能够将文本文件转译载入数据库
+ -t: 指定加密方式为hash
+ -f: 指定包含用户名密码的文本文件

修改该数据库的权限, 防止非法访问(此时vusers.list可以删除)
```bash
chmod 6000 vusers.db
```

3. 创建系统用户

创建系统用户virtual、rm并修改目录权限
```bash
useradd -d /var/ftp/test -s /sbin/nologin virtual
useradd -d /var/ftp/boss -s /sbin/nologin rm
chmod 755 /var/ftp/test
chmod 755 /var/ftp/boss
```

4. 配置PAM文件

为了让服务器能够使用数据库文件, 对客户端进行验证, 需要调用系统的PAM模块

PAM: 可拔插认证模块, 不必重新安装应用系统, 通过修改制定配置文件, 调整对程序的认证方式

PAM模块的配置文件路径为/etc/pam.d/目录

在/etc/pam.d/vsftpd.vu中写入
```txt
auth required /lib64/security/pam_userdb.so db=/etc/vsftpd/vusers
account required /lib64/security/pam_userdb.so db=/etc/vsftpd/vusers
```

5. 修改配置文件

```bash
# 注释该行
# local_root=/var/ftp/pub
# 最大客户端数
max_clients=300
# 同一个ip的用户最多可以连接的数目
max_per_ip=5
# 开启虚拟用户支持
guest_enable=YES
# 虚拟用户
guest_username=virtual
```

此时虚拟用户已经创建完成, 同时用户test、密码123和用户boss、123可以访问ftp服务器上的/var/ftp/test目录

用户test和boss都访问的是/var/ftp/test目录(本来boss应该访问/var/ftp/boss目录), 因为指定了guest_username为virtual, 此时数据库中的账户都能访问到test目录, 下面来分别指定用户的配置文件

6. 配置用户目录

在vsftpd.conf中写入
```
user_config_dir=/etc/vsftpd/vusers_dir
```

新建用户配置目录

```bash
mkdir /etc/vsftpd/vusers_dir
```

在配置目录下分别建立test和boss文件

test
```
# 开启虚拟用户支持
guest_enable=YES
# 映射的虚拟用户
guest_username=virtual
# 允许浏览服务器文件
anon_world_readable_only=NO
# 传输速率
anon_max_rate=50000
```

boss
```
# 开启虚拟用户支持
guest_enable=YES
# 映射的虚拟用户
guest_username=rm
# 允许浏览服务器文件
anon_world_readable_only=NO
# 开启写入权限
write_enable=YES
# 可以创建目录
anon_mkdir_write_enable=YES
# 可以上传文件
anon_upload_enable=YES
# 最大传输速率
anon_max_rate=100000
# 修改文件(删除文件和重命名)
anon_other_write_enable=YES
```

/etc/vsftpd/vsftpd.conf
```
local_enable=YES
anonymous_enable=NO
write_enable=YES
local_umask=022
connect_from_port_20=YES
idle_session_timeout=10
data_connection_timeout=10
chroot_local_user=YES
listen=YES
pam_service_name=vsftpd.vu
max_clients=30
max_per_ip=5
user_config_dir=/etc/vsftpd/vusers_dir
allow_writeable_chroot=YES
```

7. 查看效果

登录test用户, 可以发现用户只能浏览文件

登录boss用户, 可以发现能创建、修改文件

