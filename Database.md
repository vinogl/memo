## Mysql

### MAC memo:

* 文件地址: `/usr/local/mysql`

* 终端登录:

	* 连接远程数据库

		```
		~$ mysql -h <host> -P <port> -u <user> -p <password> 
		```

	* 连接本地的数据库

		```
		~$ mysql -uroot -p
	 	  password: vino@111
		```

- - -

## Oracle

### 实验室主机memo

* 全局数据库名: `orcl`

* SID: `ORCL`

* 管理口令: `Gongli_111`

* sys/system口令: `Gongli_111`

### 导入dmp备份文件到oracle(windows)

* 创建用户并授权:

	```
	> sqlplus / as sysdba

	SQL> create user gongli identified by 111;
	SQL> grant create session to gongli;
	SQL> grant dba to gongli;
	```
	
* 配置监听: [bilibili 参考视频](https://www.bilibili.com/video/BV1Gh41147YH)

* 导入数据(在DOS中输入，而不是SQL): 
	
	```
	> imp gongli/111@orcl full=y file='data.dmp' ignore=y;
	```

* 最后还需配置一下1521端口的防火墙！

- - -

### cx_Oracle库在Mac上配置

* 在oracle官网下载mac客户端: [下载地址](https://www.oracle.com/database/technologies/instant-client/macos-intel-x86-downloads.html)

* 解压后将指定文件拷贝到常用环境的lib文件: `/anaconda3/envs/env_GL/lib`

	```
	libclntsh.dylib
    liboramysql19.dylib
    libociei.dylib
    libnnz19.dylib
    libclntsh.dylib.19.1
    libclntshcore.dylib.19.1
	```
* 也可以在lib中创建指定文件的软链接: `~$ ln -s source(absolute path) target`
