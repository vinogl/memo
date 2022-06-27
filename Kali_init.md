# kali linux 配置

## 初始化:

### 修改管理员密码:

* 用户名/密码: `kali/kali`

* 修改root密码:

	```
	~$ sudo su
	~$ sudo passwd root
	```

### 从Xfce界面到GNOME:

```
~$ apt-get update
~$ apt-get upgrade
```

```
~$ sudo tasksel
	选择gnome界面	

~$ update-alternatives --config x-session-manager
	选择gnome界面

~$ reboot
```

### kali时间不对:

```
~$ tzselect
   [Asia - China - Beijing Time - Yes]

~$ echo "ZONE=Asia/Shanghai" >> /etc/sysconfig
~$ rm -f /etc/localtime
~$ ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 配置中文环境包、中文输入法

* 中文环境包

	```
	~$ sudo dpkg-reconfigure locales
		选择 '[ ] zh_CN.UTF-8 UTF-8'

	~$ vim /etc/profile
		添加一行 'export LANG=zh_CN.UTF-8'

	~$ reboot
	```

* 中文输入法

	```
	~$ apt-get install ibus-pinyin
	~$ im-config  # 选ibus
	~$ ibus-setup  # 打开ibus设置界面

	~$ vim .bashrc
		添加:
		export GTK_IM_MODULE=ibus
		expirt XMODIFIERS=@im=ibus
		export QT_IM_MODULE=ibus

	~$ reboot
	```

	**在'设置-键盘'只保留中文键盘，shift可切换中英文**

## 配置ip:

### 临时配置IP地址

```
~$ ifconfig eth0 192.168.1.53/24  # 临时配置ip
~$ route add default gw 192.168.1.1  # 配置默认路由(若报错，检查虚拟机的网络设置)
~$ echo nameserver 8.8.8.8 > /etc/resolv.conf  # 配置DNS服务器
```

### 永久配置IP地址

* 静如网络配置文件: 

	```
	~$ vim /etc/network/interfaces`

	添加如下内容：
    	iface eth0 inet static  # 配置eth0静态地址
    	address 192.168.1.53    # ip地址
    	netmask 255.255.255.0   # 子网掩码
    	gateway 192.168.1.1     # 网关
    ```

* 配置DNS服务: 

    ```
    ~$ vim /etc/resolv.conf  # 在文档后插入DNS地址
       nameserver 8.8.8.8
    ```

* 重启网络:
 
	```
	~$ service restart networking  # 重启网络
	```
