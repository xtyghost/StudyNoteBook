### 常用命令

#### 显示日历

```shell
cal
# 参数
-r # 显示全年日历
-j # 显示一年中第几天
```



#### 查看目录

```shell
# 查询目录中内容
ls
# 参数
-a # 显示所有文件
-l # 显示详细信息
-d # 查看目录属性
-h # 人性化显示文件大小
-i # 显示inode i节点
```

#### 切换目录

```shell
# 上次目录
cd -
# home目录
cd ~
```



#### 文件操作

```shell
# 建立目录
mkdir 目录名

# 创建文件
touch 文件名

# 参数
-p 递归创建

# 删除
rm 文件名
# 参数
-r # 删除目录
-f # 强制

# 复制
cp 源文件或目录 目标目录
# 参数
-r # 复制目录
-p # 连带文件属性复制
-d # 若源文件是连接文件，则复制链接属性
-a # 相当于 -pdr
-v # 显示复制进度

# 查找
find 查找路径 -name 文件名
find 查找路径 -iname 文件名忽略大小写
```

#### 常用目录

```shell
/bin # 命令目录
/sbin # 超级用户命令
/boot # 启动目录
/dev # 设备文件
/etc # 配置文件
/home # 普通用户家目录
/root # 超级用户家目录
/lib # 系统库
/mnt # 挂载目录
/tmp # 临时目录
/pros # 直接写入内存
/var # 系统相关文件
/usr # 系统软件资源
```



#### 查看文件

```shell
# 显示最后一屏
cat 文件名

# 回车滚动一行 空格翻页 q退出查看
more 文件名

# PgUp和PgDn翻页 q退出
less 文件名

# 显示最后几行
tail -显示最后行数 文件名
tail -f
```

#### 查看磁盘信息

```shell
df
df -H
```



#### 压缩

```shell
# 压缩
tar -zcvf 压缩后文件名 要压缩的文件

# z : 调用gzip压缩命令压缩
# c : 打包文件
# v : 显示运行过程
# f : 指定文件名
# x : 解压

# 解压
tar -xvf 压缩文件
tar -xvf 压缩文件 -C 解压目录
```

#### 进程操作

```shell
# 查看当前系统中运行的进程
ps -ef

# 结束进程
kill -9 进程PID
```

#### 关机重启

```shell
# shutdown
shutdown 参数 时间
#参数
-c # 取消前一个关机命令
-h # 关机
-r # 重启

# 其他关机
halt
poweroff
init 0

# 其他重启
reboot
init 6

# 示例
shutdown -h now # 立即关机
shutdown -h 20:20 # 20:20关机
shutdown -h +20 # 20分钟后关机
```

#### 输出重定向

```shell
# 标准输出重定向
命令 > 文件	# 覆盖文件
命令 >> 文件 # 追加文件
```

#### 多命令顺序执行

| 多命令执行符 |     格式     |     作用      |
| :----: | :--------: | :---------: |
|   ;    |  命令1;命令2   |    顺序执行     |
|   &&   |  命令1&&命令2  | 只有1正确,2才执行  |
|  \|\|  | 命令1\|\|命令2 | 只有1不正确,2才执行 |
|   \|   |  命令1\|命令2  | 管道符,2操作1结果  |

#### grep

```shell
# 搜索字符串
grep 参数 字符串 文件名
# 参数
-i # 忽略大小写
-v # 取反
```

#### awk

```shell
awk -F "指定分隔符" '条件 {指令}' 文件名
```



#### 查看当前端口使用

` netstat -an `

#### Linux权限命令

```shell
#    d       r w x      r - x      r - x
#           读 写 执行  读 写 执行   读 写 执行
# 文件类型    属主权限    属组权限    其他用户权限

# 修改权限
chmod u=rwx,g=rw,o=r 文件名
chmod 764 文件名

# 属主(user)		属组(group)	 	其他用户
# r   w   x       r   w   x       r   w   x
# 4   2   1       4   2   1       4   2   1
```

#### chattr与lsattr

```shell
# 修改属性
chattr 文件或目录
# 查看属性
lsattr 文件或目录

+参数 # 追加属性
-参数 # 移除属性
=参数 # 更新属性
# 参数
A # 文件或目录不可被修改(modified)
a # 只能向文件中添加数据，而不能删除。
i # 设定文件不能被删除、改名、设定链接关系，同时不能写入或新增内容
s # 保密性地删除文件或目录，即硬盘空间被全部收回。
```

#### SCP

```shell
# 上传文件
scp 本地文件 用户名@主机地址:目录

# 下载文件
scp 用户名@主机地址:文件 本地目录

-r # 整个目录
```

#### 防火墙配置

##### iptables

```shell
# 开放3306端口
/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT

# 将修改永久保存到防火墙
/etc/rc.d/init.d/iptables save

# 关闭防火墙
service iptables stop
# 永久关闭,开机不启动防火墙
chkconfig iptables off
```

##### firewall

```shell
# 查看防火墙状态
firewall-cmd --state
# 停止防火墙
systemctl stop firewalld.service
# 禁止防火墙开机启动
systemctl disable firewalld.service
```

#### rpm安装软件

```shell
rpm 参数 软件

# -v 显示指令执行过程
# -h或--hash 安装时列出标记
# -q 使用查询模式
# -i或--install<套件档> 安装指定套件档
# -U或--upgrade<套件档> 升级指定套件档
# -e或--erase<套件档> 删除指定套件
# --nodeps 不验证套件档的相互关联性

# 常用
rpm -ivh rpm文件 # 安装
rpm -Uvh rpm文件 # 更新
rpm -e --nodeps 软件名 # 删除
rpm -qa # 查看
```

### ab

```shell
# http压力测试
ab -n请求数量 -c并发数量 请求地址
```

### SELINUX

```shell
# 查看selinux状态
getenforce
# 关闭selinux
setenforce 0
```

### 后台运行

```shell
# 不挂断后台运行
nohup 命令 &
```

### Linux网络命名空间

```shell
# 查看命名空间
ip netns list
# 添加命名空间
ip netns add 名称
# 删除命名空间
ip netns delete 名称
# 在命名空间中执行命令
ip netns exec 名称 命令
```

### 端口占用

```shell
# 根据端口占用查找进程号
netstat -lnp | grep 端口号 | awk '{print $7}' | awk -F"/" '{print $1}'
lsof -i :端口号 | awk '{print $2}'
```

### 时间同步

```shell
yum install -y ntp \
&& sed -i "21i server cn.ntp.org.cn prefer" /etc/ntp.conf \
&& cat /etc/ntp.conf \
&& service ntpd start \
&& date \
&& sleep 5 \
&& date
```

### dig

```shell
# 安装
yum install bind-utils
# 查询
dig @DNS地址 域名
# 反向查询
dig -x DNS地址
+ short # 返回精简结果
```

### 修改编码

```shell
# 修改 /etc/locale.conf
LANG="zh_CN.UTF-8"
# 修改 ~/.bashrc
export LANG='UTF-8'
export LC_ALL='zh_CN.UTF-8'
export LC_CTYPE='zh_CN.UTF-8'
# 刷新配置
source ~/.bashrc
```

### Shell

```shell
# 显示当前shell
echo $SHELL
# 查看支持的shell
cat /etc/shells
# 切换shell
chsh -s shell名
```

