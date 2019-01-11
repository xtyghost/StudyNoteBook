### Maven仓库

+ 本地仓库 (自己维护)
+ 远程仓库 (公司维护)
+ 中央仓库 (maven团队维护)

### Maven常用命令

+ clean 清理编译好的文件
+ compile 编译主目录文件
+ test 编译并运行测试目录文件
+ package 打包项目
+ install 把项目发布到本地仓库
+ deploy 把项目发布到私有仓库

### Maven的生命周期

+ clean生命周期
  + clean
+ default生命周期
  + compile test package install deploy
+ site生命周期
  + site

不同生命周期的命令可以同时执行

```shell
# 查看版本
mvn -version
# 跳过测试
-Dmaven.test.skip=true
```

配置阿里云镜像

```xml
<mirrors>
	<mirror>
		<id>nexus-aliyun</id>
     	<mirrorOf>*</mirrorOf>
     	<name>Nexus aliyun</name>
     	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
	</mirror>
</mirrors>
```

### 搭建私有仓库

#### 使用nexus搭建

#### 修改settings配置

```xml
<settings>
	<servers> 
    	<!-- 发布到私有仓库所需配置 -->
		<server>
      		<id>与项目pom中相同</id>
      		<username>admin</username>
      		<password>admin123</password>
    	</server>
  	</servers>

  	<mirrors> 
  		<mirror>
    		<!-- 获取依赖仓库地址 -->
    		<id>nexus-public</id>
    		<mirrorOf>*</mirrorOf>
    		<url>仓库地址</url>
  		</mirror>
  	</mirrors>

  	<profiles>
    	<!-- 配置开启snapshots -->
    	<profile>
      		<activation>
        		<activeByDefault>true</activeByDefault>
      		</activation>
            <id>nexus</id>
            <repositories>
                <repository>
                    <id>maven-public</id>
                    <url>仓库地址</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                        <updatePolicy>always</updatePolicy>
                    </snapshots>
                </repository>
            </repositories>
        </profile>
  	</profiles>
</settings>

```

#### 修改pom配置

```xml
<project>
    <!--配置上传jar包到私有仓库-->
	<distributionManagement>
		<repository>
			<id>settings中server id</id>
			<name>仓库名</name>
			<url>仓库地址</url>
		</repository>
	</distributionManagement>
</project>
```

