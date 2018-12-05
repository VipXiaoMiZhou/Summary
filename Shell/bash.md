## shell

### if

```bash
#!/bin/bash

if [ $# -lt 2 ]; then
    echo "$0 arg1 arg2"
    exit 1
fi


if [[ "$1" = "hello" || "$1" = "hi" ]]; then
    echo "$1"
else
    echo "nihao"
fi

if [ $2 -gt 0 ]; then
    echo "yes"
else
    echo "no"
fi

```

### loop

```bash
#!/bin/bash

for i in $(ls); do
    echo item: $i
done


for i in `seq 1 10`; do
    echo $i
done

count=0
while [ $count -lt 20 ]; do
    echo "good "$count
    let count=count+1
done


until [ $count -lt 0 ]; do
    echo "hahah "$count
    let count=count-1
done


```

### case

```bash
#!/bin/bash

case $1 in
    "zn")
        echo "Chinese"
        ;;
    "en")
        echo "english"
        ;;
    "ja")
        echo "japen"
        ;;
    "au")
        echo "at"
        ;;
    *)
        echo "other"
        ;;
esac


```


### 计算

```bash

#!/bin/bash


count=100

let count=2*100
echo $count

let count++

while [ $count -lt 300 ]; do
    let count++
done

echo $count


while [ $count -gt 0 ]; do
    count=$[ $count-1 ]
done

echo $count

while [ $count -le 500 ]; do
    count=$((count+=2))
done

echo $count

echo --------------------bc---------------
bcount=`echo "2*30.45" |bc`
echo $bcount

echo --------------------bc---------------
bcount=`echo "scale=9;2*30.45/3;" |bc`
echo $bcount


```

### find

```bash

# find by file type and permission
find . -type d -name "*" ! -perm 777

-perm 777 #permission
find . -type d -name "*" ! -perm 777 # find directories that permission is 777

-user  #user

-group

find . -type f -name "*.sh" -perm 777 -ok bash {} \;

# delete empty files
find . -type f -name "*" -empty -exec rm -f {} \;

```

### gawk

```bash

cat /etc/passwd | gawk -F ':' 'BEGIN{size=0} {size++;} {print $5} END{print "total count is "size}'

# BEGIN 后面不能只能跟print $1 类似的语句，打不出来
cat /etc/passwd | gawk -F ':' 'BEGIN {} {print $1} END{}' 

gawk -F "=" 'BEGIN {} {if($1=="VERSION"){VERSION=$2} if($1=="NUMBER"){NUMBER=$2}} END{ print      "V="VERSION, "N="NUMBER   }' opt/config/version.csv


```

### sed

```sed```是一个用于做文件字符串的修改、添加、删除、替换等操作的命令，默认不会改变原文件的内容，而是将文件内容以流的方式处理后输送到屏幕。

**常用选项**

* -n 使用安静(silent)模式。在一般 sed 的用法中，所有来自 STDIN的资料一般都会被列出到萤幕上。但如果加上 -n 参数后，则只有经过sed 特殊处理的那一行(或者动作)才会被列出来。
* -e 直接在指令列模式上进行 sed 的动作编辑；
* -f -f filename 可以执行 filename 内的sed 动作；
* -r sed 的动作支援的是延伸型正规表示法的语法。(预设是基础正规表示法语法)
* -i 直接修改源文件       



**常用命令**

- d 删除某行

```shell
sed 'nd' Sed.py   # 删除第n行
sed '$d' Sed.py   # 删除最后一行 
sed '2,$d' Sed.py # 删除第2行到最后一行 
```

- p 显示某行,一般与-n连用

```shell
sed -n '3p' Sed.py
```

- i 插入新行

```sed '1i hello world' Sed.py # 新增一行``` 

- s 替换
```shell
sed 's/def/function/g' Sed.py   # /g 全局替换

# 注意这里的 " & " 符号，如果没有 “&”，就会直接将匹配到的字符串替换掉
sed 's/^/添加的头部&/g' 　　　　 #在所有行首添加
sed 's/$/&添加的尾部/g' 　　　　 #在所有行末添加
sed '2s/原字符串/替换字符串/g'　 #替换第2行
sed '$s/原字符串/替换字符串/g'   #替换最后一行
sed '2,5s/原字符串/替换字符串/g' #替换2到5行
sed '2,$s/原字符串/替换字符串/g' #替换2到最后一行
```

### xargs

xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令. 类似于（find ... -exec comand {} +\;）

```find /etc -name "*.conf" | xargs ls –l```
