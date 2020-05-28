在工作中往往会碰到linux操作系统，面对黑黑的命令行窗口经常不知所措，所以写一篇这样的文档用于记录平时在工作中学到的Linux 常用命令，并加以说明解释（后续不断补充）

- 常有文件需要被执行但是发现权限不够，这种情况下就需要对文件进授权 授权命令为 chmod [who] [+ |-| =] [mode] 文件名 
  
>     ```java
>     参数介绍：  
>         1.权限范围 who   
>         	u 表示“用户（user）”，即文件或目录的所有者   
>         	g 表示“同组（group）用户”，即与文件属主有相同组id的所有用户   
>         	o 表示“其他（others）用户”   
>         	a 表示“所有（all）用户”。它是系统默认值 
>         2.操作符 + - =   
>         	+ 添加某个权限   
>         	- 取消某个权限   
>         	= 赋予给定权限并取消其他所有权限（如果有的话） 
>         3.mode   
>         	r 可读   
>         	w 可写   
>         	x 可执行 
>         例如：chmod g+r，o+r example % 使同组和其他用户对文件example 有读权限
>     ```

- 常用切换用户命令，往往执行权限不够，需要用到root用户 su [userName]

>     ```java
>     总结：su 和 su - 的区别 
>          su 是切换到其他用户，但是不切换环境变量（比如说那些export命令查看一下，就知道两个命令的区别了）
>          su - 是完整的切换到一个用户环境 
>     ```

- 后台启动程序命令 nohup <程序名> & 

>     ```java
>     监控标准差输出可以用 tail -f nohup.out
>     ```

- 查看当前文件的绝对路径 pwd(选项)
>     pwd -L|P

- 文件复制，移动，删除操作

  ```java
  1.文件复制 cp [option] source1 source2 ... directory
  
  参数说明： 
      -f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制 
      -i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作 
      -p:与文件的属性一起复制，而非使用默认属性 
      -r:递归复制，用于目录的复制操作 
      -u:若目标文件比源文件旧，更新目标文件 
   例子：如将/test1目录下的file1复制到/test3目录，并将文件名改为file2 
        cp /test1/file1 /test3/file2   
  
  2.文件移动 mv [-fiv] source destination
  
  参数说明：
  
  	-f:force，强制直接移动而不询问
  	-i:若目标文件(destination)已经存在，就会询问是否覆盖
  	-u:若目标文件已经存在，且源文件比较新，才会更新
  例子：如将/test1目录下的file1移动到/test3 目录，并将文件名改为file2
  	mv /test1/file1 /test3/file2
  
  3.文件删除 rm [fir] 文件或目录
  
   参数说明：
  	 -f:强制删除
  	 -i:交互模式，在删除前询问用户是否操作
   	 -r:递归删除，常用在目录的删除
   例子：如删除/test目录下的file1文件
   	 rm -i /test/file1
   4.不同服务器之间文件复制
          
  ```
  
- 防火墙操作

  关闭防火墙（以centos6.5为例）
  查看防火墙状态 service iptables status

  关闭（打开）防火墙 
  永久生效
  chkconfig iptables off（on）
  重启时效
  service iptables stop(start)


- 修改ip地址
>     修改eth0文件
>     1.编辑网卡文件 vi /etc/sysconfig/network-script/ifcfg-eth0
>     2.修改ip为静态 BOOTPROTO="static"
>     3.添加ip IPADDR=192.168.100.251
>     4.需要修改mak地址的可以 注释掉 HWADDR 添加 MACADDR="00:0C:29:B3:40:11"
>     5.重启网卡服务 service network restart


- 主机名和域名修改
  
>     1.修改计算机名称
>     1.打开配置文件 vi  /etc/sysconfig/network
>     2.设置HOSTNAME=计算机名称
>     3.reboot 
>  
>     2.修改域名解析
>     查看当前主机名 hostname
>     1.修改hosts文件 vi /etc/hosts 在末尾加上 ip地址或者127.0.0.1 域名
>     2.退出保存即可，测试 ping 域名