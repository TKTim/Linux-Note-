操作環境:
VM1, VM2, VM3
伺服器:
VM1:
客戶端:
VM2, VM3

### 架設概念圖:

<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/1.JPG"  width="600" height="650">

### VM1網路設置如下: (須設定三張網路卡)
<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/2.png"  width="600" height="500">
<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/3.png"  width="600" height="500">
<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/4.png"  width="600" height="500">






### VM2, VM3的網路設置: (都各只需要一張網路卡)
<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/5.png"  width="600" height="500">


# 那就開始吧!!!!


### 1:
```
VM1:
# ip addr add 192.168.1.1/24 brd + dev <網卡名稱>

舉例// ip addr add 192.168.1.1/24 brd + dev enp0s9 
(因為我是第三張)



VM2:
# ip addr add 192.168.1.1/24 brd + dev <網卡名稱>

舉例// ip addr add 192.168.1.1/24 brd + dev enp0s3 
(因為我是第一張)
# ip route add default via 192.168.1.254
#ifconfig
//確認是否更改完成


VM3: (注意看 ip尾是2喔)
# ip addr add 192.168.1.2/24 brd + dev <網卡名稱>

舉例// ip addr add 192.168.1.2/24 brd + dev enp0s3 
(因為我是第一張)
# ip route add default via 192.168.1.254
#ifconfig
//確認是否更改完成

```

### 2.
```
VM1:
#echo 1 > /proc/sys/net/ipv4/ip_forward
啟動路由功能
#vim /etc/sysctl.conf
---
net.ipv4.ip_forward = 1
---
#sysctl -p
net.ipv4.ip_forward = 1
```
### 3.
```
VM1: 下載

#yum install ipvsadm -y
```

### 若是沒辦法下載

```
更改 /etc/resolv.conf
增加 
nameserver 8.8.8.8
即可
```

### 4.
```
# ipvsadm -A -t 192.168.67.100:80 -s rr
#  ipvsadm -a -t 192.168.67.100:80 -r 192.168.1.1:80 -m
# ipvsadm -a -t 192.168.67.100:80 -r 192.168.1.2:80 -m

#  curl 192.168.67.100:80/a.htm

若有錯誤，表示你的port號並不是80，
到 VM2,VM3 虛擬機中
# cd /etc/httpd/conf
# vim httpd.conf 
----------------------
Listen的部分要設定成80。
```
<img src="https://github.com/TKTim/Linux-Note-/blob/master/05-27--%E9%99%84%E8%BC%89%E5%9D%87%E8%A1%A1--ipvsadm/6.png"  width="600" height="500">
