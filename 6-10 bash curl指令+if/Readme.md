1. 建立httpd
=

```
# yum install httpd
# cd /var/www/html 
# vim hi.htm 
---------------
Hello You are right
---------------
# systemctl start httpd
```


2. 創立 mycurl.sh
=

[root@node02 Downloads]# vim mycurl.sh

```
#!/bin/bash

ok=/tmp/ok.txt

fail=/tmp/fail.txt

>$ok
>$fail

for i in {1..100}  #嘗試100次

do

        curl --connect-timeout 1 http://192.168.56.103/hi.htm &>/dev/null  #curl

        if [ $? -eq 0 ]; then  #If success do = If  0 do

                echo "curl ok" >> $ok #成功時輸入
        else
                echo "curl fail" >> $fail #失敗時輸入
        fi

done

echo "all finish..."
if [ $? -eq 0 ]; then   #若成功就印出網頁內容
        curl  http://192.168.56.103/hi.htm
fi
yes=`wc -l $ok`
no=`wc -l $fail`
echo "ok:" $yes
echo "fail:" $no

```

3. Result
=
```
# ./mycurl.sh
```


















