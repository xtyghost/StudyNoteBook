## 配置环境变量

### 配置文件

#### 根据Shell不同配置不同文件

+ bash : vim ~/.bash_profile
+ zsh : vim ~/.zshrc

#### 刷新配置

source ~/.zshrc

### 配置多个JDK环境变量

```shell
# 配置jdk7环境变量
export JAVA_7_HOME='/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home'
# 配置jdk8环境变量
export JAVA_8_HOME='/Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home'
# 配置默认为jdk7
export JAVA_HOME=$JAVA_7_HOME

# 配置快速切换jdk环境变量
alias jdk7="export JAVA_HOME=$JAVA_7_HOME"
alias jdk8="export JAVA_HOME=$JAVA_8_HOME"
```

### 配置maven环境变量

```shell
# 配置maven环境变量
export MAVEN_HOME=/Users/zhangaochong/Library/apache-maven-3.3.9
export PATH=$PATH:$MAVEN_HOME/bin
```

### 终端SS代理

```shell
# 配置终端ss代理
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1086"
alias unsetproxy="unset ALL_PROXY"
```

## 其他

### 使用iterm + expect自动连接服务器

```shell
# 安装expect
brew install expect
# 编写脚本
#!/usr/bin/expect -f
# 自动登陆服务器脚本
# 使用: 脚本 用户名 地址 端口 密码
spawn ssh [lindex $argv 0]@[lindex $argv 1] -p [lindex $argv 2]
expect {
	"(yes/no)?"
	{send "yes\n";exp_continue}
	"password:"
	{send "[lindex $argv 3]\r"}
}
expect "#" {send "clear\r"}
interact
```
