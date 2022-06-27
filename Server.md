## 服务器

- - -

### 平台

* [kamatera](https://console.kamatera.com/)

	* 账号: `libing0720@gmail.com`
	
	* 密码: `Vps770720103`
	
	* 手机号: `13810912942`

* [Orcle Cloud](https://www.oracle.com)

	|账号名|用户名|密码|地区|
	|:--:|:--:|:--:|:--:|
	|`kk163`|`kk163@sohu.com`|`Vps@7707kk163`|Korea Chuncheon|
	|`q1448`|`814484812@qq.com`|`Vps@8144`|US SanJose|
	|`k5767`|`35767988@qq.com`|`Vps@3576`|Singapore|
	|`m3420602`|`33420602@qq.com`|`Vps@3342`|Sydney|

### 服务器信息

|标识|二级域名|ip|登录用户|密码or密钥|
|:--:|:--:|:--:|:--:|:--:|
|`kk163-1`|`koreaone`|`152.69.234.32`|`root`|`~/.ssh/kk163-1.key`|
|`kk163-2`|`koreatwo`|`146.56.97.75`|`root`|`~/.ssh/kk163-2.key`|

### Xray-ui界面

|所属服务器|地址|
|:--:|:--:|
|`kk163-1`|`koreaone.meta99.ga:2048`|
|`kk163-2`|`koreatwo.meta99.ga:2048`|

- - -

## 获取域名

* [freenom](https://my.freenom.com):

	|邮箱|密码|
	|:--:|:--:|
	|`yh907104235@gmail.com`|`Gl907104235`|
	|`libing2008@yandex.com`|`Ym770720`|

	映射: 
	
	|域名|ip|
	|:--:|:--:|
	|`koreaone.meta99.ga`|`152.69.234.32`|
	|`koreatwo.meta99.ga`|`146.56.97.75`|

* [namesilo](https://www.namesilo.com):
	
	用户名: `Vino_gl`
	
	账号: `yh907104235@gmail.com`

	密码: `Gl@907104235`

	获取的域名: `vpserver.xyz`
	
- - -

## 服务器配置安装的server

### 流媒体解锁检测

```
~$ bash <(curl -sL https://git.huaweicdn.net/media_unlock_check.sh)
```

### 初始化操作([参考](https://github.com/bigdongdongCLUB/GoodGoodStudyDayDayUp/issues/7))

* 开启root登录:

	```
	~$ sudo su

	~$ vim /root/.ssh/authorized_keys
		把ssh-rsa之前的文件都删除掉(不删除‘ssh-rsa’)

	~$ vim /etc/ssh/sshd_config
		找到 PermitRootLogin
		添加 PermitRootLogin yes

	~$ reboot
	```

* 开启防火墙:

	```
	~$ iptables -P INPUT ACCEPT
	~$ iptables -P FORWARD ACCEPT
	~$ iptables -P OUTPUT ACCEPT
	~$ iptables -F
	~$ apt-get purge netfilter-persistent

	~$ reboot
	```

* 配置BBR加速:

	```
	~$ echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
	~$ echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
	~$ sysctl -p
	~$ lsmod | grep bbr
	```
	> 打完最后一步，应看到 20480 或 16384 说明BBR开启成功

- - -

### 在服务器上配置Xray-ui界面([参考](https://github.com/bigdongdongCLUB/GoodGoodStudyDayDayUp/issues/8))

* 申请SSL证书:
 
	```
	~$ apt update -y
	~$ apt install -y curl
	~$ apt install -y socat
	```
	```
	~$ curl https://get.acme.sh | sh
	~$ ~/.acme.sh/acme.sh --register-account -m <邮箱>

	~$ ~/.acme.sh/acme.sh --issue -d <域名> --standalone

	~$ ~/.acme.sh/acme.sh --installcert -d <域名> --key-file /root/private.key --fullchain-file /root/cert.crt
	```
	> 完成后，root目录下看到证书公钥/root/cert.crt及密钥文件/root/private.key

* 安装、配置X-ui面板:

	* 安装: 

	```
	~$ bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
	
	账户名: admin
	设置密码: 907104235
	访问端口: 2048(随意选)
	```

	* 登录: `domain: port`

	* 进入'面板设置':
		
	```
	添加'公钥路径' -> /root/cert.crt
	添加'私钥路径' -> /root/private.key
	点击'重启面板'
	```

- - -

### 订阅转换服务的搭建([参考](https://codeswift.top/posts/docker-subscription-converter/))

* docker安装subconverter:

	```
	~$ sudo docker run -d --restart=always --name subconverter -p 25500:25500 tindy2013/subconverter:latest

	~$ curl http://localhost:25500/version  # 验证安装是否成功
	```

* docker搭建前端sub-web:

	* 复制项目到本地，并修改后端地址

		```
		~$ git clone https://github.com/CareyWang/sub-web.git
		~$ cd sub-web
		~$ vim .env  # 修改API后端
		```

	* 在项目主目录下，构建并运行该项目:

		```
		~$ sudo docker build -t subweb-local:latest .
		~$ sudo docker run -d -p 58080:80 --restart always --name subweb subweb-local:latest
		```
		> 上述 docker run 命令将容器的 80 端口映射到了主机的 58080 端口。
