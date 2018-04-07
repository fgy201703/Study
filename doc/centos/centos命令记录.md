## centos命令常用记录

### 1，文件及目录操作命令

* mkdir   kk                   即创建文件 kk

* rm -rf                          即删除文件或 删除目录及子目录下所有目录及文件

* cp   kk.txt   k/k1.txt   即将 kk.txt 复制到当前k目录中并且命名为 k1.txt 

* mv  kk.txt  k2.txt       即将kk.txt 改名为k2.txt  

* touch                          生一个空文件或更改文件时间

* mv  kk.txt  k/    即将kk.txt 移动到当前k目录下

* ls     -a   查看隐藏文件   -l  查看文件权限   -R  查看当前目录及目录中的文件  

* ll      查看文件及目录  占用大小

* ln     kk.txt   1   即将 kk.txt 和1文件进行硬关联    参数 -s  即软关联

* vi     文件编辑相关命令

  ​     i  进入编辑状态    q  进入命令状态 

  ​    命令状态  删除一行  按d  

* cat 、more 、head、tail -n  查看文件内容命令 

* find  / -name   kk.txt  / 在根目录下查找kk.txt文件

* grep  fsa 1     即查找 文件1中包含  fsa 的信息

* fdisk -l            查看磁盘信息


###  2，文件系统权限命令

*  chmod   kk.txt 777  文件权限变更命令  

  各个权限的意思如下：

  1，第一个字母d表示是目录,-表示的是文件。

  2，rwx分别表示读写和执行权限。

  3，3组的rwx分别表示当前用户、组用户和其他用户的执行权限。

    说明：  rwx也可以用数字来代替  r -------4    w -------2    x -------1  -  -------0  

  +表示添加权限      -表示删除权限  = 表示使之成为唯一的权限  


  -rw-------    (600) 只有所有者才有读和写的权限  


  -rw-r--r--    (644) 只有所有者才有读和写的权限，组群和其他人只有读的权限  


  -rwx------    (700) 只有所有者才有读，写，执行的权限  


  -rwxr-xr-x    (755) 只有所有者才有读，写，执行的权限，组群和其他人只有读和执行的权限  


  -rwx--x--x    (711) 只有所有者才有读，写，执行的权限，组群和其他人只有执行的权限  


  -rw-rw-rw- (666) 每个人都有读写的权限  


  -rwxrwxrwx (777) 每个人都有读写和执行的权限  

* chown yfg  kk.txt   将文件所有者修改为用户yfg

  ​

  ​

  详见  https://blog.csdn.net/qq_27376871/article/details/51967219 有详细比较


### 3，管道

* 分页查看   如   ls -Rl /etc | more   即查看 etc 目录及子目录下的所有文件  分页显示

  cat /etc/passwd | wc                  显示passwd文件中 有多少行

  cat /etc/passwd | grep w           显示passwd文件中带有w的信息

  dmesg | grep eth0                      查找网卡的启动信息

  ​

  man bash | col -b  > bash.txt    将 man bash 信息中过滤掉所有的控制字符，包括RLF和HRLF；写入                          到 bash.txt文件中 

  ls -l |grep "^d"                           查找有 d 权限的目录

  ls -l * |grep "^-" |wc -l  

  ​

* 命令替换    向所有用户发送信息命令：  wall date    命令替换

         wall   aaaa