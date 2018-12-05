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

### vue-router

#### 动态路由

```javascript
// 路由模式
mode: 'history'
mode: 'hash'

// 配置路由
path: '/路径/:参数1/路径/:参数2...'

// 获取路由参数
$route.params.参数名
// 获取查询参数
$route.query.参数名
```

#### 嵌套路由

```javascript
// 配置子路由
children: [
    子路由1,
    子路由2,
    ...
]
```

```html
<!-- 跳转子路由 -->
<router-link to='子路由路径'></router-link>

<!-- 显示子路由 -->
<router-view></router-view>
```

#### 编程式路由

```javascript
// 跳转
this.$router.push('路径')
this.$router.go(跳转)
```

#### 命名路由

```vue
<router-link v-bind:to='{name: 路由名},params:{参数名:参数值}'></router-link>
```

#### 命名视图

```javascript
components: {
    default: 默认视图,
    视图: 视图名
    ...
}
```

```vue
<router-view name=视图名 />
```

### vue resource

```shell
# 安装
npm i vue-resource --save
```

```javascript
this.$http.get(url,{
    params: {},
    headers: {}
}).then()
```

