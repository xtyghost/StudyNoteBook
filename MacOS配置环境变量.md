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

