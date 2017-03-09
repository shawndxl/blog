### sh

### awk

### grep

### `` 变量

### nginx

* conf

* -t -s

### redis

### mkdir touch vi -d -q -!q -wq // gitlab的教训告诉我们最好不要学习删除指令

### sudo

### yum


实例 读取nginx日志, 计算近一分钟内访问某地址的平均响应时间，并拼接key存储在redis中，供后续项目取key值并展现在页面中用于实时监控
---------

> 实例-nginx log：10.33.**.*** - - [15/Feb/2017:14:19:48 +0800] "GET /Share/Link/id/1000/red_num/1 HTTP/1.0" section.baidu.com 200 2015 "-" "Dalvik/2.1.0 (Linux; U; Android 6.0.1; MI MAX MIUI/V7.3.12.0.MBCCNDC)" "139.214.254.44""-" "200" "unix:/dev/shm/php7-cgi.sock" "0.045" "0.041"

```shell
log_path=/data/logs/getShareLink.log
searchWord1=Link

curDate=`date`
searchWord2=`date -d "$curDate -1 minute" "+%d/%b/%Y:%H:%M"` # 15/Feb/2017:14:19 最分的分钟是是当前的分钟-1S
keyDate=`date -d "$curDate -1 minute" "+%Y%m%d%H%M"` # 201702151002 最分的分钟是是当前的分钟-1S
rediskey=h5:get_share_link:$keyDate
exp=259200 # 三天
# exp=20

echo '查询路径: '$log_path
echo '查询字: '$searchWord1', '$searchWord2
echo 'key: '$rediskey
echo '过期时间: '$exp

request_time_avg=`tail -n 100000 $log_path | # 从近**条数据中
grep $searchWord1 -m 50000 | # 找到符合关键字1的 最大**条
grep $searchWord2 -m 50000 | # 找到符合关键2的  最大**条
awk '{print $(NF - 1)}' | # 取倒数第二个关键字既响应时间 单位 S
sed 's/"//g' |
awk '{sum += $1} END {print sum / NR}'`

echo '值: '$request_time_avg

redis-cli -h 127.0.0.1 -p 6379 -n 0 setex $rediskey $exp $request_time_avg
```
