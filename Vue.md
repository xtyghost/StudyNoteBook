### NPM

```shell
# 安装插件
npm install 插件名
# 查看已安装插件
npm list
```

### Vue

```shell
# 安装Vue
npm install vue --save
```

#### 开发步骤

```shell
# 初始化项目
vue init 模版 目录名
# 安装插件
npm install
# 运行项目
npm run 启动脚本
```

#### 项目结构

+ build 打包配置文件
  + webpack.base 打包核心配置
+ config webpack配置
  + index.js 
+ node_modules
+ src 源码
+ static 静态资源
+ babelrc ES6解析配置
+ package 基础配置
  + script 脚本命令
  + dependencies 依赖
  + devDependencies 开发依赖
  + engines node引擎
  + browserslist