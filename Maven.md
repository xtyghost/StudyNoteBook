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

### Maven的生命周期

+ clean生命周期
  + clean
+ default生命周期
  + compile test package install deploy
+ site生命周期
  + site

不同生命周期的命令可以同时执行