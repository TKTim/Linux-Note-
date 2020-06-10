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
<img src="https://github.com/TKTim/Linux-Note-/blob/master/06-10/1.png"  width="800" height="800">
<img src="https://github.com/TKTim/Linux-Note-/blob/master/06-10/2.png"  width="600" height="400">

記得將Docker改成上次安裝的檔案，還沒裝的輸入:
```
#docker pull smallko/php-apache-dev:v10
```

下載haproxy
=
安裝流程
```
# apt-get update
# apt-get install haproxy
```
解釋:
>HAProxy是一個使用C語言編寫的自由及開放原始碼軟體，其提供高可用性、負載均衡，以及基於TCP和HTTP的應用程式代理。
GitHub、Bitbucket、Stack Overflow、Reddit、Tumblr、Twitter和 Tuenti在內的知名網站，及亞馬遜網路服務系統都使用了HAProxy。

awk,gred,sed Linux三元素 [研究網站](https://codertw.com/%E5%89%8D%E7%AB%AF%E9%96%8B%E7%99%BC/392291/)
=
```
root/~#sed 
//stream editor
不須將檔案打開，就能做出新增、修改、查詢。
root/~#sed -i
//-i 修改內容
#sed -i '/^$/d' 檔案名稱
// 刪除空行
```

刪除空行
=

```
#sed -i '/^$/d' 檔案名稱
// 刪除空行
```

## 前:

<img src="https://github.com/TKTim/Linux-Note-/blob/master/06-10/3.png"  width="700" height="700">

## 後:

<img src="https://github.com/TKTim/Linux-Note-/blob/master/06-10/4.png"  width="700" height="700">

問題: 取出一般使用者名稱
=

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
但是我們只要使用者名稱。 

<kbd>grep "/bin/bash"</kbd>, 選出有相符字串

<kbd>grep -v "root"</kbd> , --invert-match        select non-matching lines

<kbd>awk -F: '{print $1}' </kbd>, 顯示第一位值

<kbd>awk -F</kbd>, fs    --field-separator=fs


```
root@p4:/home/p4/test-lvs# cat /etc/passwd | grep "/bin/bash" | grep -v "root" | awk -F: '{print $1}'
vagrant
p4
```
補充
=

* sed -i '20d' a.txt     // 刪除第20行
* sed -i '10,20d' a.txt // 刪除10到第20行


* sed -i '/^ntp/d' a.txt // 刪除開頭為ntp的行數

* sed -i '10,/^user/d' a.txt // 刪除從第10行到開頭為user的行數

a: 增加 ， 轉譯
=
```
a: 增加
sed '1a drink tea' 檔案名稱 //第一行後增加字串"drink tea"
sed -i '$a do' 檔案名稱     //在最後一行加do




轉譯:(想刪除/*為開頭的字串)
sed -i '/^\/*/d' 檔案名稱
若想處理的字串中有 "  /   "，會與我們的處理符號 "   //   "搞混，因此多增加"  \   "
1. sed -i '/^/d'   //鎖定開頭，刪除符號
2. sed -i '/^/*/d' //加入想刪除的字符
3. sed -i '/^\/*/d' //在字符前加上 "  \   " 
```
s: 取代 ，g:不斷匹配
=
```
sed -i 's/p4/P4' 檔案名稱 // 將P4取代掉p4
sed -i 's/p4/P4/g' 檔案名稱 //若沒有g，則指取代找到的第一行
sed -i 's/p4/P4/g'

sed -i '/do/s/do/does/g' ggg.txt  //找出do將do換成does
```
