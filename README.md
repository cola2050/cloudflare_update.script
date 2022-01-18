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

1、获取 CF 的 ID：

curl -X GET "https://api.cloudflare.com/client/v4/zones/这里填你官网的 Zone ID/dns_records" \
     -H "X-Auth-Email: 这里填你登录的 EMAIL 例如：xxx@gmail.com" \
     -H "X-Auth-Key: 这里填你的 API Keys 例如：c9a3a22e788cafcd827b78e1e8dfa7f22b370" \
     -H "Content-Type: application/json"
这里注意下，每个子域名获取的 id 都是不一样的:


2、修改完毕，把script更新到ros需要添加如下执行权限：

*Read

*Write

*Test

*Sniff

*Sensitive

3、在ROS的system/scheduler添加一个轮训的计划任务：比如每五分钟执行一次

至此脚本全部设置完毕，看看是否更新。
