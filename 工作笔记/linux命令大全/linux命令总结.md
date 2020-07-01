在工作中往往会碰到linux操作系统，面对黑黑的命令行窗口经常不知所措，所以写一篇这样的文档用于记录平时在工作中学到的Linux 常用命令，并加以说明解释（后续不断补充）

### 查看版本

```
uname -a

Linux localhost.localdomain 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
```

查看CUP

```
[root@localhost ~]# lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    1
Core(s) per socket:    4
Socket(s):             2
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 62
Stepping:              4
CPU MHz:               2593.750
BogoMIPS:              5187.50
Hypervisor vendor:     VMware
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              20480K
NUMA node0 CPU(s):     0-7
```

查看内存

```
cat /proc/meminfo |grep MemTotal
```

查看硬盘

```
fdisk -l |grep Disk
```



### 主机名和域名修改

​	**1.修改计算机名称**

```
vi /etc/sysconfig/network #打开编辑

HOSTNAME=Centons7.yaoshuaishuai.com #设置hostname

reboot #重启
```

​	**2.修改域名解析**

​		查看当前主机名 hostname

```
vi /etc/hosts #编辑修改hosts文件 

127.0.0.1 域名 #在末尾加上域名映射


ping 域名 #测试是否成功

reboot #重启
```





### 修改ip地址

​	修改lo 文件

```
DEVICE=lo #网卡名称

IPADDR=172.16.192.34 #IP地址

GATEWAY=172.16.192.1 #网关

NETMASK=255.255.244.0 #子网掩码

BROADCAST=172.16.192.255 #广播

NETWORK=yes #网络是否被配置

ONBOOT=yes #启动时网卡是否激活

ONBOOT=yes #是否随着开机自启动 

BOOTPROTO=static  #static表示固定ip地址，dhcp表示随机获取ip  

重启网络设置：service network restart

systemctl restart network

systemctl status network.service
```



### 防火墙操作

​	**1.关闭防火墙（以centos6.5为例）**

```
查看防火墙状态 ：service iptables status

关闭（打开）防火墙 

永久生效：chkconfig iptables off（on）

重启失效：service iptables stop(start)
```

​	**2.关闭防火墙（以centos7为例）**

```
查看防火墙状态：firewall-cmd --state

关闭（打开）防火墙

关闭防火墙：systemctl stop firewalld.service

禁止防火墙开机自启：systemctl disable firewalld.service

防火墙重加载：firewall-cmd --reload
```

​	**3.开闭端口操作**

```
查看所有开启的端口号：netstat -aptn 

查看系统中所有使用udp协议的端口号：netstat -nupl

查看系统中使用tcp协议的端口号信息：netstat -ntpl

​	添加开放端口号：firewall-cmd --zone=public --add-port=80/tcp --permanent

	命令含义：

​	--add-port=80/tcp #添加端口，格式为：端口/通讯协议

​	--permanent #永久生效，没有此参数重启后失效
```



### 赋权： 授权命令为 chmod [who] [+ |-| =] [mode] 文件名 

​	**1.给文件添加某个权限**

```
参数介绍： 

①权限范围 who   

​    u 表示“用户（user）”，即文件或目录的所有者   

​    g 表示“同组（group）用户”，即与文件属主有相同组id的所有用户   

​    o 表示“其他（others）用户”   

​    a 表示“所有（all）用户”。它是系统默认值 

②操作符 + - =   

③mode   r 可读   w 可写   x 可执行 

例如：chmod g+r，o+r example % 使同组和其他用户对文件example 有读权限
```

​	**2.给文件夹及文件修改属组**

```
	chmod 用户名：用户名 文件夹/或者名称

​	例如：chmod abase610:abase610 arteybase610 
```



### 切换用户：需要用到root用户 su [userName]

```
总结：su 和 su - 的区别 

​     su 是切换到其他用户，但是不切换环境变量（比如说那些export命令查看一下，就知道两个命令的区别了）

​     su - 是完整的切换到一个用户环境 
```



### 后台启动程序命令： nohup <程序名> & 

```
nohup 命令 & #后台命令启动服务

tail -f nohup.out #监控标准差输出可以用
```



### 查看当前文件的绝对路径： pwd(选项)

```
pwd -L|P
```

### 文件复制，移动，删除操作

​	**1.文件复制 cp [option] source1 source2 ... directory**

```
参数说明： 

​    -f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制 

​    -i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作 

​    -p:与文件的属性一起复制，而非使用默认属性 

​    -r:递归复制，用于目录的复制操作 

​    -u:若目标文件比源文件旧，更新目标文件 

	例子：如将/test1目录下的file1复制到/test3目录，并将文件名改为file2 

​     cp /test1/file1 /test3/file2   
```

​	**2.文件移动 mv [-fiv] source destination**

```
参数说明：

​	-f:force，强制直接移动而不询问

​	-i:若目标文件(destination)已经存在，就会询问是否覆盖

​	-u:若目标文件已经存在，且源文件比较新，才会更新

	例子：如将/test1目录下的file1移动到/test3 目录，并将文件名改为file2

​	mv /test1/file1 /test3/file2
```

​	**3.文件删除 rm [fir] 文件或目录**

```
参数说明：
	-f:强制删除

​	-i:交互模式，在删除前询问用户是否操作

​	-r:递归删除，常用在目录的删除

	例子：如删除/test目录下的file1文件

​	rm -i /test/file1
```

​	 **4.不同服务器之间文件复制**

```
①把当前主机copy到远程主机

​	scp /home/a.txt  root@192.168.0.8:/home/root 复制文件

②把远程主机copy到当前主机

​	scp root@192.168.0.8:/home/b.txt  /home/root #复制文件

​	scp -r root@192.168.0.8:/home/ /root/home2 #复制文件夹
```









