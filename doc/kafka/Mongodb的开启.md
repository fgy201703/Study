**Mongodb的开启**

默认启动：

$ ./mongodb

默认数据保存路径：/data/db/

默认端口：27017

**修改默认路径：**

--dbpath

$ ./mongdb --dbpath /mongodb/

把数据存储位置指向一个自己的目录/mongodb/

**修改默认端口：**

--port

$ ./mongdb --port 20111

把服务端口修改为20111，这个一方面是为了安全，使用默认端口容易被一些恶意的人发现做手脚

**启动后台服务:**

--fork

在后台开启Mongdb服务

在使用这个方式启动的时候要注意两点：

1、该功能只在1.1之后的版本才可以使用。

2、另外通过这个方式在后台启动，如果在启动的时候像--dbpath 那样使用 --logpath 输出日志时候日志输出目录也要自己创建。

如：

$ ./mongod --fork --logpath /var/log/mongodb.log --logappend

解析最后--logappend,以追加的方式创建日志防止把之前的日志删除了

**Mongodb的关闭:**

**前台运行:**

如果没有使用--fork，直接可以前台退出终端关闭。通过这种方式，Mongodb将会自己做清理退出，把没有写好的数据写完成，并最终关闭数据文件。要注意的是这个过程会持续到所有操作都完成。

**后台运行:**

如果使用--fork在后台运行mongdb服务，那么就要通过向服务器发送shutdownServer()消息来关闭。

1、普通命令：

$ ./mongod

\> use admin

\> db.shutdownServer()

要注意的是，这个命令只允许在本地，或是一个经过认证的客户端。

2、如果这是一个主从式的复制集群，在1.9.1版本后将按下面的步骤来关闭

- 检查从Mongodb的数据更新时间
- 如果所有的从Mongodb和主的时间差都超过10，这个时候不会关闭mongodb（在这种情况下面，我们可以通过配置timeoutSecs的方式来让从Mongodb完成数据的更新）
- 如果其中有一个从Mongodb与主服务时间差在10秒内，那么主服务器将会关闭，并且等待从Mongodb更新完成并关闭。

3、如果没有up-to-date 从Mongodb且你想强制关闭服务，可以通过添加force:true;命令如下：

> db.adminCommand({shutdown : 1, force : true})

> //or

> db.shutdownServer({force : true})

4、指定特定超时时间的关闭服务器，命令同上，另外加上一个timeoutsec:参数

> db.adminCommand(shutdown : 1, force : true, timeoutsec : 5)\> [//or]

> db.shutdownServer({force : true, timeoutsec : 5})

**Mongodb开机启动**

在/etc/rc.local文件末尾添加下面的代码

\#add mongodb service

rm -rf /data/mongodb_data/* && /usr/local/mongodb/bin/mongod --dbpath=/data/mongdb_data/ --logpath=/data/mongdb_log/mongodb.log --logappend &