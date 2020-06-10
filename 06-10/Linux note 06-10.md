下載mycurl.sh到p4
-
以下有註解
1.開啟mininet by 
```
#cd /test-lvs
#python 1.py
```
```
root@p4:/home/p4/Downloads# cat mycurl.sh
#!/bin/bash


ok=/tmp/ok.txt

fail=/tmp/fail.txt

>$ok

>$fail


for i in {1..100}

do

        curl --connect-timeout 1 http://10.0.1.254/a.htm &>/dev/null

        if [ $? -eq 0 ]; then  #If success do = If  0 do

                echo "curl ok" >> $ok   #put result in /temp/ok

        else

                echo "curl fail" >> $fail

        fi

done


echo "all finish..."

yes=`wc -l $ok`

no=`wc -l $fail`

echo "ok:" $yes

echo "fail:" $no
```

建立[test-haproxy.py] & [haproxy.cfg]
-
![image1]()
![image2]()

記得將Docker改成上次安裝的檔案，還沒裝的輸入:
```
#docker pull smallko/php-apache-dev:v10
```

下載haproxy
-
安裝流程
```
# apt-get update
# apt-get install haproxy
```
解釋:
>HAProxy是一個使用C語言編寫的自由及開放原始碼軟體，其提供高可用性、負載均衡，以及基於TCP和HTTP的應用程式代理。
GitHub、Bitbucket、Stack Overflow、Reddit、Tumblr、Twitter和 Tuenti在內的知名網站，及亞馬遜網路服務系統都使用了HAProxy。

awk,gred,sed Linux三元素
-
```
root/~#sed 
//stream editor
不須將檔案打開，就能做出新增、修改、查詢。
root/~#sed -i
//-i 修改內容
#sed -i '/^$/d' 檔案名稱
// 刪除空行
```
>問題: 取出一般使用者名稱
```
root@p4:/home/p4/test-lvs# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
以下省略...
```
但是我們只要名稱。 
grep "/bin/bash" , 選出有相符字串

grep -v "root" , --invert-match        select non-matching lines

awk -F: '{print $1}', 顯示第一位值

awk -F, fs                   --field-separator=fs


```
root@p4:/home/p4/test-lvs# cat /etc/passwd | grep "/bin/bash" | grep -v "root" | awk -F: '{print $1}'
vagrant
p4
```






















