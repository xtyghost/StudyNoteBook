#### 初始化控件

```java
// 返回view对象
findViewById(R.id.控件ID)
```

#### Activity生命周期

+ onCreate
+ onStart
+ onResume
+ onPause
+ onStop
+ onDestory

### ADB

```shell
# 安装
brew install android-platform-tools
# 查看设备列表
adb devices

# 无线ADB
# 开启服务-使用USB
adb tcpip 5555
# 开启服务-手机端执行
su
stop adbd
start adbd
setprop service.adb.tcp.port 5555
# 连接手机
adb connect 手机ip
# 断开连接
adb disconnect 手机ip
```

