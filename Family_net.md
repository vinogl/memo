## wifi设置

### 网络拨号

* 账号: `730052056714`

* 密码: `123456`

### 路由器

|设备信息|无线wifi|后台管理地址|后台权限|
|:--:|:--:|:--:|:--:|
|`R2S`|作为`gbly上级路由`|`192.168.2.1`|`root`/`password`|
|`TL-WDR5620`|`gbly`/`13874035999`|`192.168.11.1`|`password`|
|`NetGear R6300`|`liumeng65kg`/`liumeng65kg`|`10.0.0.1`|`admin`/`password`|
|`TL-WR842N`|`lygb`/`ly13974030766`|`192.168.14.1`|`password`|

### 折腾路由器

* [`OpenWrt`初始化设置](https://www.youtube.com/watch?v=_i7ip8n-f5o)

* [`OpenWrt`旁路由科学上网](https://www.youtube.com/watch?v=P6NdEjycHhw&t=699s)

* 在`OpenWrt`上使用`Docker`部署简历

	* [OpenWrt给docker扩容](https://wxf2088.xyz/2949.html)

	* 将`html`上传系统并改名
		
	```
	~$ scp /CV/CV.html root@192.168.2.1:/root/dist
	~$ mv /root/dist/CV.html /root/dist/index.html 
	```

	* `docker`-`nginx`部署静态网页

	```
	~$ docker pull ngnix  # 第一次先下载ngnix镜像
	~$ docker run -d -p 2048:80 -v /root/dist:/usr/share/nginx/html nginx 
	```

	* 查看`192.168.2.1:2048`

### 折腾`Google TV`

* `Google Tv`初始化连不上wifi

```
进入OpenWrt后台 -> 网络 -> DHCP/DNS -> 自定义挟持域名

	time.android.com - 203.107.6.88
```

* wifi网络受限: 修改`ntp_server`([视频参考](https://www.youtube.com/watch?v=G6aJGeeB30k))

	