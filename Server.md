## 服务器

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

* [AWS](https://aws.amazon.com)

	* 用户名: `vino`

	* 账号: `gongli.vino@gmail.com`

	* 密码: `Gl@907104235`

### 服务器信息

|标识|ip|登录用户|密码or密钥|Xray界面url|
|:--:|:--:|:--:|:--:|:--:|
|`kk163-1`|`152.67.199.163`|`root`|`~/.ssh/kk163-1.key`|`koreaone.meta99.ga:2048`|
|`kk163-2`|`150.230.250.160`|`root`|`~/.ssh/kk163-2.key`|`koreatwo.meta99.ga:2048`|
|`aws-US-CA`|`54.193.65.171`|`root`|`~/.ssh/aws-us-ca.pem`|`usca.meta99.ga:2048`|

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
	|`koreaone.meta99.ga`|`152.67.199.163`|
	|`koreatwo.meta99.ga`|`150.230.250.160`|
	|`usca.meta99.ga`|`54.193.65.171`|

* [namesilo](https://www.namesilo.com):
	
	用户名: `Vino_gl`
	
	账号: `yh907104235@gmail.com`

	密码: `Gl@907104235`

	获取的域名: `vpserver.xyz`
	
- - -
- - -

## 服务器配置安装的server

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

### 流媒体解锁

* Netflix解锁

	* warp([github](https://github.com/fscarmen/warp))

		```
		~$ wget -N https://raw.githubusercontent.com/fscarmen/warp/main/menu.sh
		~$ bash menu.sh
		```

	* [视频参考](https://www.youtube.com/watch?v=5DedN1O_f2E&t=71s)

* 流媒体解锁检测

	```
	~$ bash <(curl -sL https://git.huaweicdn.net/media_unlock_check.sh)
	```

### 配置Xray-ui界面([参考](https://github.com/bigdongdongCLUB/GoodGoodStudyDayDayUp/issues/8))

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

### 订阅转换服务的搭建(见`Docker.md`)
