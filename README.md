# cloudflare_update.script

此脚本为Mikrotik RouterOS update script for CloudFlare
使用方法：

1、查看脚本需要如下參數：
:local CFDebug "true"
:global WANInterface "ether1-gateway"  

:local CFdomain "sub.domain.com"
:local CFzone "domain.com"

:local CFemail "email@example.com"
:local CFtkn "YOUR_API_KEY"

:local CFzoneid "YOUR_ZONE_ID"
:local CFid "YOUR_ID"
注意這裏需要先执行下面代码先获取到CFID：
curl -X GET "https://api.cloudflare.com/client/v4/zones/YOUR_ZONE_ID/dns_records" -H "X-Auth-Email: YOUR_EMAIL" -H "Authorization:Bearer YOUR_API_KEY" -H "Content-Type: application/json" | python -mjson.tool
2、修改完毕，把script更新到ros需要添加如下执行权限：
*Read
*Write
*Test
*Sniff
*Sensitive
3、在ROS的system/scheduler添加一个轮训的计划任务：比如每五分钟执行一次

至此脚本全部设置完毕，看看是否更新。
