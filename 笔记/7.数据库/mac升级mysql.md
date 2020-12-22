# mac升级mysql版本，并迁移数据

https://www.jb51.net/os/MAC/355066.html

**在苹果Mac OS X 系统上升级 Mysql 数据库步骤**

**1、首先停止 Mysql 服务**

sudo /usr/local/mysql/support-files/mysql.server stop

**2、然后下载你需要的 Mysql 安装包。**如果你之前有启动项 与 偏好设置安装了 。 那只需要安装第一个数据库的安装包即可。

安装好以后你文件会存储在。

/usr/local/mysql-8.0.20-macos10.15-x86_64

并且 mysql 的链接会指向同样的位置

/usr/local/mysql

而你之前的数据库应在在同样的位置

/usr/local/mysql-5.7.26-macos10.14-x86_64

**3、现在我们要做的就是替换数据库文件  data 文件夹。** 首先将新数据库文件夹改名

sudo mv /usr/local/mysql-8.0.20-macos10.15-x86_64/data /usr/local/mysql-8.0.20-macos10.15-x86_64/dataold

**4、然后将老的数据库目录的数据库文件复制过去**

sudo cp -rf /usr/local/mysql-5.7.26-macos10.14-x86_64/data /usr/local/mysql-8.0.20-macos10.15-x86_64/

**5、然后设置正确的权限**

sudo chown -R _mysql /usr/local/mysql-8.0.20-macos10.15-x86_64/data

**6、启动Mysql 然后修复数据库**

sudo /usr/local/mysql/support-files/mysql.server start

**7、运行升级程序**

/usr/local/mysql/bin/mysql_upgrade

**8、如果出现错误就再运行一次，随后重启 Mysql 服务**

sudo /usr/local/mysql/support-files/mysql.server restart

**9、查看新的版本号**

/usr/local/mysql/bin/mysql

**10、重新设定root 密码**

/usr/local/mysql/bin/mysqladmin -u root password 'yourpasswordhere'



启动不成功，点击了初始化mysql，结果系统便好设置里面的图标不见了

使用命令启动

`ctydeMacBook-Pro:/ cty$ /usr/local/mysql/bin/mysqld_safe --skip-grant-tables`

报错：

```
ctydeMacBook-Pro:/ cty$ /usr/local/mysql/bin/mysqld_safe --skip-grant-tables
/usr/local/mysql/bin/mysqld_safe: line 674: /usr/local/mysql/data/ctydeMacBook-Pro.local.err: Permission denied
Logging to '/usr/local/mysql/data/ctydeMacBook-Pro.local.err'.
2020-05-19T02:35:56.6NZ mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data
/usr/local/mysql/bin/mysqld_safe: line 144: /usr/local/mysql/data/ctydeMacBook-Pro.local.err: Permission denied
/usr/local/mysql/bin/mysqld_safe: line 199: /usr/local/mysql/data/ctydeMacBook-Pro.local.err: Permission denied
/usr/local/mysql/bin/mysqld_safe: line 937: /usr/local/mysql/data/ctydeMacBook-Pro.local.err: Permission denied
rm: /usr/local/mysql/data/ctydeMacBook-Pro.local.pid.shutdown: Permission denied
2020-05-19T02:35:56.6NZ mysqld_safe mysqld from pid file /usr/local/mysql/data/ctydeMacBook-Pro.local.pid ended
/usr/local/mysql/bin/mysqld_safe: line 144: /usr/local/mysql/data/ctydeMacBook-Pro.local.err: Permission denied
```



常见报错：

```
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)

ERROR 2003 (HY000): Can't connect to MySQL server on '127.0.0.1' (61)
```



重新安装，发现data目录还在,执行5，6，7命令，报错：

```
The mysql_upgrade client is now deprecated. The actions executed by the upgrade client are now done by the server.
To upgrade, please start the new MySQL binary with the older data directory. Repairing user tables is done automatically. Restart is not required after upgrade.
The upgrade process automatically starts on running a new MySQL binary with an older data directory. To avoid accidental upgrades, please use the --upgrade=NONE option with the MySQL binary. The option --upgrade=FORCE is also provided to run the server upgrade sequence on demand.
It may be possible that the server upgrade fails due to a number of reasons. In that case, the upgrade sequence will run again during the next MySQL server start. If the server upgrade fails repeatedly, the server can be started with the --upgrade=MINIMAL option to start the server without executing the upgrade sequence, thus allowing users to manually rectify the problem.
```

发现[*MySQL* 8.0.16开始 *mysql_upgrade* 升级程序已经废弃 ](https://www.baidu.com/link?url=38vZBb0MT8eIV6lZylFHO-AA22OYSrdS48_2h2AMY5FGhi_cPJoV8vydui1XDUaoLuDG5n7Hh0Krm2CaSn_5uq&wd=&eqid=ebacb113000020cc000000045ec349dc)

登陆mysql，成功。验证数据，没问题，数据已迁移。

可视化工具无法连接到mysql。

1.修改mysql端口号

2.禁止mysql验证功能



[macbook中出现2003 - Can't connect to MySQL server on '127.0.0.1' (61 "Connection refused") 如何解决](https://www.cnblogs.com/Jokerguigui/p/11724356.html)

第一步　　关闭mysql服务:

```
苹果->系统偏好设置->最下边点mysql 在弹出页面中 关闭mysql服务（点击stop mysql server）
```

如果这种方法没有成功:

可以使用命令行关闭mysql:

```
~$ sudo /usr/local/mysql/support-files/mysql.server stop
```

第二步　　

1、进入终端输入：cd /usr/local/mysql/bin

2、车后 登录管理员权限 sudo su  （输入你电脑的密码）

3、回车后输入以下命令来禁止mysql验证功能  ./mysqld_safe --skip-grant-tables（注意是mysqld）

4、回车后mysql会自动重启（偏好设置中mysql的状态会变成running）



第三步

1、输入命令 ./mysql

2、回车后，输入命令 FLUSH PRIVILEGES;
3、回车后，输入命令 ALTER user 'root'@'localhost' IDENTIFIED BY '123456'  （123456,这是新密码随意写一个记住的）

第四步

重启mysql:

```
~$ sudo /usr/local/mysql/support-files/mysql.server restart
```