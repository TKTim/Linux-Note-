本日筆記
=

### p1.sh

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
![]()
