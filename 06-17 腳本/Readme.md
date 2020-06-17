本日筆記
=

### p1.sh

##### 由於
```
[root@vm1 Downloads]# uname -i
x86_64
```
因此我們可以利用這行指令，做出相對應的sh。

```
#!/bin/bash

platform=`uname -i`
if [ $platform != "x86_64" ];then
  echo "this script is only for 64bit OS"
  exit 1
fi

echo "the platform is ok"
cat << EOF
+=======================================+
| your system is CentOS 7 X86_64        |
| start optimizing........              |
+=======================================+
EOF
```

### 輸出結果:
![mg](https://github.com/TKTim/Linux-Note-/blob/master/06-17%20%E8%85%B3%E6%9C%AC/1.png)


設定hinet網路依據
=

可以指定以後安裝、更新，都使用國內的源
```
cd /var/www/html
vim CentOS-Base.repo
```

### 輸入:

```
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates 
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
baseurl=http://mirror01.idc.hinet.net/CentOS/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```
完成後儲存!

再輸入:

```
# wget http://127.0.0.1/CentOS-Base.repo
這樣便從剛剛的位置下載下來檔案了。
# yum install htop
這邊先下載htop做測試。
可以下載就代表沒問題了，但由於Taiwan是個自由的國家，所以速度上的激情? 是感覺不太到的。
```

p1.sh 進化版
=

再之前的檔案增加這幾行:

```
# yum -y install ntp
下載ntp
# timedatectl set-timezone Asia/Taipei
設定為台北時區
/usr/sbin/ntpdate  tock.stdtime.gov.tw

echo "* 4 * * * /usr/sbin/ntpdate tock.stdtime.gov.tw > /dev/null 2>&1" >> /var/spool/cron/root
在固定時間，更新時間。
systemctl restart crond.service
重新開啟crond
```
校對時間的資料: [NTP同步](https://blog.gtwang.org/linux/linux-ntp-installation-and-configuration-tutorial/)

































