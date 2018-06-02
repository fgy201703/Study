##nexus私服搭建

1，先下载nexus的应用地址：

​     https://www.sonatype.com/download-oss-sonatype

​     根据需要选择自己操作系统对应的版本

2，解压对应的版本

3，安装jdk

​      注：本文所有提及JDK的地方可等价替换为JRE，因为nexus只需JRE即可，你也可以下载单独的JRE安装包,3.xx需要jdk1.8及以上，并且是Oracle Java,其他发行版不支持，包括OpenJDK。    

1. sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_65/bin/javac"    
2.  && sudo update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_65/bin/javac  
3.  && sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_65/bin/java"    
4.  && sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_65/bin/java 

4，安装nexus

​     解压安装包

​    tar -zxvf /tmp/nexus-3.0.1-01-unix.tar.gz -C /var/lib 

​    做个连接，方便日后升级    

​    cd /var/lib  

​    sudo ln -s nexus-3.0.1-01 nexus3 

 5，修改Nexus运行用户

   在nexus /bin/nexus中的脚本



​    RUN_AS_USER=root  #修改后的内容，代表nexus私服使用root用户权限

   在3.x中 是小写 run_as_user=root



6，修改防火墙 开方nexus私服端口访问   8081

​      vi /etc/sysconfig/iptables

​      将22号端口复制一份出来修改为8081

​     再重启 service iptables restart

7,启动

   ./nexus start

  命令  /nexus status  状态   stop  停止

http访问    192.168.58.130:8081/nexus

注意进入时为下图点击红色框进入配置页面，至于报404 暂未找到日志文件 不清楚什么原因

![](D:\study\github\github-track\doc\git\nexus-image\进入页面.png)

进入后可正常访问

![](D:\study\github\github-track\doc\git\nexus-image\进入主页面.png)



8，配置

​      登录 账号  admin  admin123   进入配置页面     ![](D:\study\github\github-track\doc\git\nexus-image\配置页面.png)

​     在security 中查看用户信息   和权限信息

​    注意3是要点击上面的齿轮进入配置页面 



9，检查仓库

​    在vies/repositories

​    中的repositories

​    常用的仓库类型：hosted 和proxy 

​    hosted 主要用于发布内部项目构件或第三方的项目构件

​        releases 发布模块

​        snapshots 快照

​        3rd party 第三方依赖仓库 通常由内部人员自行下载之后发布上去   nexus3 中没有此仓库

  proxy 将远程仓库代理

![1527323112931](D:\study\github\github-track\doc\git\nexus-image\仓库页面.png)



10，central 中央仓库   

​      将 configuration 中的downloadremote index  设置为true  允许远程下载

​     自行下载

####意下面几点说明：

1.component name的一些说明： 
    1）maven-central：maven中央库，默认从https://repo1.maven.org/maven2/拉取jar 
    2）maven-releases：私库发行版jar 
    3）maven-snapshots：私库快照（调试版本）jar 
    4）maven-public：仓库分组，把上面三个仓库组合在一起对外提供服务，在本地maven基础配置settings.xml中使用。
2.Nexus默认的仓库类型有以下四种：
    1）group(仓库组类型)：又叫组仓库，用于方便开发人员自己设定的仓库；
    2）hosted(宿主类型)：内部项目的发布仓库（内部开发人员，发布上去存放的仓库）；
    3）proxy(代理类型)：从远程中央仓库中寻找数据的仓库（可以点击对应的仓库的Configuration页签下Remote Storage Location属性的值即被代理的远程仓库的路径）；
    4）virtual(虚拟类型)：虚拟仓库（这个基本用不到，重点关注上面三个仓库的使用）；
3.Policy(策略):表示该仓库为发布(Release)版本仓库还是快照(Snapshot)版本仓库；
4.Public Repositories下的仓库 
   1）3rd party: 无法从公共仓库获得的第三方发布版本的构件仓库，即第三方依赖的仓库，这个数据通常是由内部人员自行下载之后发布上去；
   2）Apache Snapshots: 用了代理ApacheMaven仓库快照版本的构件仓库 
   3）Central: 用来代理maven中央仓库中发布版本构件的仓库 
   4）Central M1 shadow: 用于提供中央仓库中M1格式的发布版本的构件镜像仓库 
   5）Codehaus Snapshots: 用来代理CodehausMaven 仓库的快照版本构件的仓库 
   6）Releases: 内部的模块中release模块的发布仓库，用来部署管理内部的发布版本构件的宿主类型仓库；release是发布版本；
   7）Snapshots:发布内部的SNAPSHOT模块的仓库，用来部署管理内部的快照版本构件的宿主类型仓库；snapshots是快照版本，也就是不稳定版本
所以自定义构建的仓库组代理仓库的顺序为：Releases，Snapshots，3rd party，Central。也可以使用oschina放到Central前面，下载包会更快。
5.Nexus默认的端口是8081，可以在etc/nexus-default.properties配置中修改。
6.Nexus默认的用户名密码是admin/admin123
7.当遇到奇怪问题时，重启nexus，重启后web界面要1分钟左右后才能访问。
8.Nexus的工作目录是sonatype-work（路径一般在nexus同级目录下）
[root@master-node local]# pwd
/usr/local
[root@master-node local]# ls nexus/
bin deploy etc lib LICENSE.txt NOTICE.txt public system
[root@master-node local]# ls sonatype-work/
nexus3
[root@master-node local]# ls sonatype-work/nexus3/
backup blobs cache db elasticsearch etc generated-bundles health-check instances keystores lock log orient port tmp



11，私服应用

​      在maven中的settings.xml 修改

​     <server>​          

​     </server>    

















 