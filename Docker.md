## Docker

> 安装docker引擎([参考](https://docs.docker.com/engine/install/))

### `docker`常用命令

	```
	~$ docker ps  # 查看所有容器
	~$ docker rm -f <容器id(不用输全)>  # 删除容器
	~$ docker exec -it <容器id(不用输全)> bash  # 进入容器内部
	~$ exit  #  退出容器
	```

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


### 使用`ngnix`部署静态网页

```
~$ docker pull ngnix  # 第一次先下载ngnix镜像
~$ docker run -d -p 80:80 -v /dist:/usr/share/nginx/html nginx  # 将/dist指定为外部文件
```

>`/usr/share/nginx/html`为容器内部`html`所在地址 <br>
> 将`html`文件放到`/dist`下，并命名为`index.html`
>
> 若不命名为`index.html`
>
> 进入容器 <br>
> `/etc/nginx/nginx.conf`为配置文件地址 <br> 
> 修改`/etc/nginx/conf.d`中的`default.conf`

