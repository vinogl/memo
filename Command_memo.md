## command memo

* [chmod](https://www.runoob.com/linux/linux-comm-chmod.html): 控制用户对文件的权限的命令

```
~$ chmod mode <file>

	mode: 权限设定字串，格式见链接文档
```

* [grep](https://www.runoob.com/linux/linux-comm-grep.html): 查找文件里符合条件的字符串
	
```
~$ grep <string> <file>  # 在file下查找string
```  

* [curl](https://www.cnblogs.com/duhuo/p/5695256.html): 利用URL规则在命令行下工作的文件传输工具

* [wget](https://www.jianshu.com/p/59bb131bc2ab): 下载文件的工具

```
~$ apt/yum/brew install wget  # 系统默认不带wget

~$ wget [option] [url]
```

* rsync: 文件同步 （windows: [robocopy](https://www.uc23.net/command/130.html)）

```
~$ rsync [options] <source> <destination>

	source, destination可为ssh远程文件: user_name@remote_ip:file_path
```

* [scp](https://www.runoob.com/linux/linux-comm-scp.html):  Linux之间复制文件和目录

```
~$ scp [-P port] <file_source> <file_target>
	
	<file_source>: [[user@]host1:]file1
	<file_target>: [[user@]host2:]file2
```

* iptables: [25 个最常用的iptables规则示例](https://bbs.huaweicloud.com/blogs/300487)

* [redir](https://github.com/troglobit/redir)(系统默认不带)

```
~$ redir <:port> <ip(or domain)>:<port>  # 将指定端口流入流量转发
```


* Linux服务配置

	* operation: `start|stop|reload|force-reload|restart|try-restart|status`

	* server: `networking | ssh | apache2 | firewalld.service｜ ...`

```
~$ systemctl <operation> <server>    # 重启网络
~$ service <operation> <server>      # 重启网络
~$ /etc/init.d/<server> <operation>  # 重启网络
```

## Docker

* [安装docker引擎](https://docs.docker.com/engine/install/)

* `docker`常用命令

```
~$ docker ps  # 查看所有容器
~$ docker rm -f <容器id(不用输全)>  # 删除容器
~$ docker exec -it <容器id(不用输全)> bash  # 进入容器内部
~$ exit  #  退出容器
```

* [订阅转换服务的搭建](https://codeswift.top/posts/docker-subscription-converter/)(`Server.md`中有记录)

* 使用`ngnix`部署静态网页

```
~$ docker pull ngnix  # 第一次先下载ngnix镜像
~$ docker run -d -p 80:80 -v /dist:/usr/share/nginx/html nginx  # 将/dist指定为外部文件
```

>`/usr/share/nginx/html`为容器内部`html`所在地址 <br />
> 将`html`文件放到`/dist`下，并命名为`index.html`
>
> 若不命名为`index.html`
>
> 进入容器 <br />
> `/etc/nginx/nginx.conf`为配置文件地址 <br /> 
> 修改`/etc/nginx/conf.d`中的`default.conf`

## conda常用命令

```
~$ conda init zsh  # 初始化激活base环境(在'~/.zshrc'下生成脚本)
~$ conda create -n <env_name> python=<版本号>  # conda终端创建新环境
~$ source(or conda) activate <env_name>  # 进入环境
```

## ssh远程登录命令

* 密钥登录
```
~$ ssh -i <private_key_file> <username>@<ip>

	第一次先给私钥设置权限: ~$ chmod 400 <private_key_file>
```
* 密码登录
```
~$ ssh <user>@<ip>
```

* `Linux`-`ssh`服务

	* `ssh`服务端管理

	```
	~$ service ssh start  # 启动ssh
	(start|stop|reload|force-reload|restart|try-restart|status)  # 所有可选命令
	```
	
	* 允许root登录ssh服务
	
	```
	~$ vim /etc/ssh/sshd_config  

		修改35和40行:
		PermitRootLogin yes
		PubkeyAuthentication yes

	~$ update-rc.d ssh enable  # 设置开机启动sshd服务
	```
