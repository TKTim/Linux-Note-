操作環境:
VM1, VM2, VM3
伺服器:
VM1:
客戶端:
VM2, VM3

### LVS(Linux Virtual Server)
> 如架設概念圖，可以看到 LVS擔任主機搭載了負載均衡的功能，而要實作它，便要用到Linux目前內設的ipvsadm
這邊有一些介紹的網站，裡面也包括了指令集
[參考網站一](https://www.cnblogs.com/klb561/p/9215704.html)，
[參考網站二](https://www.jianshu.com/p/20f034f8c076)。

### 在這裡給出一些常看到的指令:
```
-A --add-service添加一條新的虛擬服務
-t | -uTCP / UDP協議的虛擬服務
-s配置負載均衡算法，如：rr，wrr，lc等
RR:
輪詢調度(Round Robin 簡稱'RR')算法就是按依次循環的方式將請求調度到不同的服務器上，該算法最大的特點就是實現簡單。輪詢算法假設所有的服務器處理請求的能力都一樣的，調度器會將所有的請求平均分配給每個真實服務器。
```



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
