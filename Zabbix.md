# zabbix配置

* [密码](#密码)

* [实用命令](#实用命令)

* [自定义key](#自定义key)

	* [Linux端配置方法](#Linux端配置方法)

	* [Windows端配置方法](#Windows端配置方法)

	* [Server端命令执行key](#Server端命令执行key)

* [自定义监控项](#自定义监控项)
* [SNMP监控](#SNMP监控)

* [遇到的问题](#遇到的问题)

	* [Zabbix服务器端运行中---否](#Zabbix服务器端运行中---否)

	* [zabbix-server前端乱码](#zabbix-server前端乱码)

	* [zabbix_agent启动失败](#zabbix_agent启动失败)

* [服务端配置](#服务端配置)

- - -

<span id="密码"></span>
## 密码:

* 登录项：

|   |用户|密码|
|:--:|:--:|:--:|
|CentOS|`root`|`TwgdhXjdmg`|
|zabbix web|`Admin`|`zabbix`|

* mysql_secure:

|项目|密码|
|:--:|:--:|
|root password|`TwgdhXjdmg`|
|zabbix password|`TwgdhXjdmg`|

- - -

<span id="实用命令"></span>
## 实用命令

* netstat (*用于显示网络状态*)

	* 服务端查看状态: `~$ netstat -tunlp`

	* -h获取命令提示: `~$ netstat -h`

<span id="自定义key"></span>
## 自定义key

<span id="Linux端配置方法"></span>
### Linux端配置方法 (以“监控登录服务器人数”为例)

1. 明确agent端执行的linux命令: `~$ who ｜ wc -l`

2. agent端添加自定义key配置:

	* 在何处添加配置:

		```
		~$ vim /etc/zabbix/zabbix_agentd.conf  # 在‘主配置文件’添加配置

		/Option: UserParameter  # 该内容下能看到定义key的语法
		Format: UserParameter=<key>,<shell command>
	
		/Include=		
		Include=/etc/zabbix/zabbix_agentd.d/*.conf  # 也可添加到该附加文件中
		```

	* 动手定义: `UserParameter=login.user,who ｜ wc -l`

	* 重启`zabbix-agent`: `~$ systemctl restart zabbix-agent.service`

<span id="Windows端配置方法"></span>
### Windows端配置方法 (以“当前登录用户”为例)

1. 明确agent端执行的cmd命令: `> whoami` (`query user`)

2. agent端添加自定义key配置:

	* 在何处添加配置: `C:\Program Files\Zabbix Agent2`
	
		(同上，可以在zabbix_agent2.conf里直接添加，也可以在附近文件zabbix_agent2.conf.d添加)
		
	* 在配置文件`zabbix_agent2.conf`中‘取消注释’或‘添加’如下字段:

		```
		Include=\zabbix_agent2.conf.d\*conf  # zabbix_agent配置文件地址		
		UnsafeUserParameters=1  # 允许使用特殊字符
		```

	* 动手定义: `UserParameter=login.user,whoami`

	* 重启 `zabbix-agent2.exe`:
		
		```
		> cd \Zabbix Agent 2  # 进入zabbix_agent文件目录
		> zabbix_agent2.exe -c zabbix_agent2.conf -x  # 停止zabbix_agent
		> zabbix_agent2.exe -c zabbix_agent2.conf -s  # 启动zabbix_agent
		
		或者:
		
		win+R - services.msc: 找到Zabbix Agent
		```

<span id="Server端命令执行key"></span>
### Server端命令执行key

* 第一次，先下载`zabbix_agent`: `yum install zabbix-get.x86_64`

* 执行命令: `~$ zabbix_get -s '127.0.0.1' -p 10050 -k 'login.user'`

- - -

<span id="自定义监控项"></span>
## 自定义监控项

1. 创建模版

2. 创建应用集(**应用集内放监控项**)

3. 创建监控项，自定义item

4. 创建触发器，当监控项获取值后，和触发器比较，判断是否需要报警

5. 创建图形

6. 将具体的主机和该模版链接、关联

- - -

<span id="SNMP监控"></span>
## SNMP监控

* 客户端 (在cisio交换机上配置snmp):

	```
	C:> telnet host_ip
	Password: ******

	> enable
	# configure terminal
	(config)# snmp-server community GokuBlack ro
	(config)# snmp-server contact Zamasu <zamasu@dbsuper.com>
	(config)# snmp-server location Universe10 - IT Room
	(config)# exit
	#
	# copy running-config startup-config  # 保存交换机配置(非必需操作)
	```

* 服务端:

	1. 服务端安装snmp监控程序:

		`~$ yum -y install net-snmp net-snmp-utils`

	2. 开启snmp配置:
		
		1. 在snmp.conf'第57行添加'view systemview included  .1': `~$ sed -i.ori '57a view systemview included  .1' /etc/snmp/snmp.conf`

		2. 启动snmp服务: `~$ systemctl start snmpd.service`

	3. 使用snmp命令:
	
		`~$ snmpwalk -v2c -c public host_ip sysname`
		
		`~$ snmpwalk -v2c -c GokuBlack host_ip`

		* -v 指定协议版本

		* -c 指定暗号

		* sysname snmp的key

- - -

<span id="遇到的问题"></span>
## 遇到的问题

<span id="Zabbix服务器端运行中---否"></span>
### Zabbix服务器端运行中---否

*. 关闭SELINUX

	* 临时关闭

		```
		~$ getenforce
		Enforcing
		~$ setenforce 0
		~$ getenforce
		Permissive
		```

	* 永久关闭

		```
		~$ vim /etc/sysconfig/selinux
		SELINUX=enforcing 改为 SELINUX=disabled
		~$ reboot
		```

*. 内存不够，修改参数

	`~$ vim /etc/zabbix/zabbix_server.conf`

	**修改: **`CashSize=256M` (or big)
	
- - -

<span id="zabbix-server前端乱码"></span>
### zabbix-server前端乱码 (*缺少中文字体*)

1. 将Win10操作系统`C:\Windows\Fonts`目录下的任意字体，拷贝到`/usr/share/zabbix/assets/fonts/`目录下，更改后缀名`.ttc`为`.ttf`

2. 修改php脚本文件:`vim /usr/share/zabbix/include/defines.inc.php`

	**修改81行和123行如下:**

	```
	define('ZBX_GRAPH_FONT_NAME', 'msyh'); // font file name

	define('ZBX_FONT_NAME', 'msyh');
	```
	 
- - -

<span id="zabbix_agent启动失败"></span>
### zabbix_agent启动失败

* 检查配置文件，将Zabbix Agent安装为Windows系统的服务: `> zabbix_agentd.exe -i -c zabbix_agentd.conf`

<span id="服务端配置"></span>
## 服务端配置

1. 获取zabbix下载源:

	`~$ rpm -Uvh https://mirrors.aliyun.com/zabbix/zabbix/5.0/rhel/7/x86_64/zabbix-release-5.0-1.e17.noarch.rpm`

2. 在样本仓库中可以看到`zabbix.repo`: 

	`~$ ls /etc/yum.repos.d/`

3. 更换zabbix.repo的yum源: 

	`~$ sed -i 's#http://repo.zabbix.com#https://mirrors.aliyun.com/zabbix#' /etc/yum.repos.d/zabbix.repo`
	
	##### 可以看到zabbix.repo中镜像源改变了: `$ vim /etc/yum.repos.d/zabbix.repo`

4. 清空yum缓存，下载zabbix:

	`~$ yum clean all`

	`~$ yum install zabbix-server-mysql zabbix-agent -y`

5. 安装`scl`:

	`~$ yum install centos-release-scl -y`

	**可以在机器上，使用多个版本的软件，并且不会影响到整个系统，安装内容放在:** `/opt/rh/package-name/`

6. 修改zabbix-front前端源:

	`~$ vim /etc/yum.repos.d/zabbix.repo` 

	**修改:** `[zabbix-fronted]  enable=1`

7. 安装zabbix前端环境，安装到scl环境下:

	`~$ yum install zabbix-web-mysql-scl zabbix-apache-conf-scl -y`

	**查看安装:** `~$ ls /opt/rh/`

	**存在文件:** `~$ rh.php72`

8. 安装zabbix需要的数据库，mariadb:

	`~$ yum install mariadb-server -y`

9. 配置数据库，开机启动:

	`~$ systemctl enable --now mariadb`
	
10. 初始化数据库，设置密码:

* 查看状态:
 
	```
	~$ systemctl status mariadb

	~$ netstat -tunlp
	```

* 初始化: 

	`~$ mysql_secure_installation`
	
	```
	> Set root password? [Y/n] y
	> New password: TwgdhXjdmg
	
	> R emove anonymous users? [Y/n] y
	
	> Disallow root login remotely? [Y/n] n

	> Remove test database and access to it? [Y/n] y

	> Reload privilege tables now? [Y/n] y
	```
		
11. 添加数据库用户，已经zabbix所需的数据库信息:
 
	 **以root身份登录测试: **`~$ mysql -uroot -p`

	```
	> Enter password: TwgdhXjdmg
	
	> create database zabbix character set utf8 collate utf8_bin;

	> create user zabbix@localhost identified by 'TwgdhXjdmg';

	> grant all privileges on zabbix.* to zabbix@localhost;

	> flush privileges;

	> exit;
	```

12. 使用zabbix-mysql命令，导入数据库信息:

	`zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix`
	
	`Enter password: TwgdhXjdmg`

	**检查数据库:**
	
	**以root身份登录测试:** `mysql -uzabbix -pTwgdhXjdmg`

	```
	> show databases;

	> use zabbix

	> show tables;

	> exit;
	```

13. 修改zabbix server配置文件，修改数据库密码:

	`~$ vim /etc/zabbix/zabbix_server.conf`

	```
	:/DBPassword
	
	DBPassword: TwgdhXjdmg
	```
	
	**检查:** `~$ grep '^DBPa' /etc/zabbix/zabbix_server.conf`

14. 修改zabbix的php配置文件:

	`~$ vim /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf`

	```
	:/timezone

	php_value[date.timezone] = Asia/shanghai
	```

	**检查:** `~$ grep 'timezone' /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf`

15. 启动zabbix相关服务:

	```
	~$ systemctl restart zabbix-server zabbix-agent httpd rh-php72-php-fpm
	~$ systemctl enable zabbix-server zabbix-agent httpd rh-php72-php-fpm
	```
