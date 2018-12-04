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
