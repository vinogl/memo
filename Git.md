# git

### 同步本地到`Github`/`Gitee`

```
~$ git init                          # 生成.git隐藏文件
~$ git add .                         # 将文件上传到暂存区
~$ git commit -m 'first commit'      # 提交到本地仓库
~$ git branch -M master
~$ git remote add vino <url>
~$ git push -u vino master           # 推送(第一次)
```

### `git`与本地同步常用命令

```
~$ git status                    #  查看当前状态

~$ git add .                     # 将文件的修改上传到暂存区
~$ git commit -m '<note>'        # 提交到本地仓库
~$ git push                      # 推送(非第一次推送)

~$ git pull                      # 将远程代码同步到本地
```

### `git`-`branch`操作常用命令

```
~$ git branch [-v] [-vv] [-a] [-r]              # 查看分支信息
~$ git branch -m/M [旧分支] <新分支>              # 对已经存在的branch重名(大写为强制)

~$ git branch <分支名>                           # 创建本地分支
~$ git checkout <分支名>                         # 切换本地分支
~$ git checkout -b <分支名>                      # 创建本地分支并切换

~$ git branch --delete(or -d/D) <分支名>         # 删除本地分支(大写为强制)
~$ git push <主机名> --delete(or -d/D) <分支名>   # 删除远程分支(大写为强制)

~$ git push <远程主机名> <本地分支名>:<远程分支名>   # 将本地分支推送到远程分支(远程分支不存在，则创建)

~$ git merge <其他分支>                          # 将<其他分支>合并到当前分支
```

### `Github Pages`(托管页面)

1. 进入`GitHub`页面

2. `Settings` -> `Pages`
	
3. `Branch`: `None`改为`master`, 保存


### `github`添加`token`（[官方文档](https://docs.github.com/cn/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)）

* 创建`token`

	1. 进入`GitHub`页面

	2. `Settings` -> `Developer settings` -> `Personal access tokens`

* `Macbook`使用`token`登录

	1. 打开`钥匙串访问`应用

	2. 搜索`github`，双击编辑，修改密码

	> 如果还没之前没有登录过，在终端进行`git`操作触发登录

### `git`配置

* 配置文件级别

|级别|`key`|配置文件所在地址(`mac`)|
|:--:|:--:|:--:|
|仓库级别|`--local`|`当前仓库/.git/config`|
|用户级别|`--global`|`~/.gitconfig`|
|系统级别|`--system`|`/etc/gitconfig`<br />(macbook上没找到)|

* `config`命令

```
~$ git config <key> --list(or -l)        # 列出所有 Git 能找到的配置
~$ git config <key> user.name "Gong Li"  # 设置变量
~$ git config <key> --unset user.name    # 取消变量
```

### `Macbook`上`.DS_Store`隐藏文件

* 可直接删除某文件夹内的所有`.DS_Store`
	
```
~$ find ./ -name .DS_Store | xargs rm -rf
```

* `Macbook`配置忽略`.DS_Store`

	1. 创建`~/.gitignore_global`文件，把需要全局忽略的文件写入该文件

	```
	~$ vim ~/.gitignore_global

		写入:
		.DS_Store
		 */.DS_Store
	```

	2. 在`~/.gitconfig`中引入`.gitignore_global`文件，手动写入`~/.gitconfig`，或输入如下代码
	```
	~$ git config --global core.excludesfile /Users/GongLi/.gitignore_global
	```	

