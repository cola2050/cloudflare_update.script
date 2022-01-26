# cloudflare_update.script
此脚本为Mikrotik RouterOS update script for CloudFlare

前提条件

注意：脚本是基于RouterOS v6.46.4 编写的，大于小于此版本都可能导致一些命令问题

确认正确的公网地址

IPv4验证方法：

脚本提取 (WinBox –> IP –> Address List) 内指定接口的IP地址进行解析

ROS终端运行：/ip address get [/ip address find interface=接口名称] address

IPv6验证方法：

脚本提取 DHCPv6 Client 获取的 Prefix 并加上指定的IPv6后缀进行解析

ROS终端运行：/ipv6 dhcp-client get [find interface=接口名称] status

查看读出的数据是否为公网地址，
提前新建子域名
在 CloudFlare 新建需要解析的子域名，若需要解析IPv6和双栈还需要建立IPv4同名子域名和单独子域名，单独子域名用于IPv6是否更新的判断
ipv4.hscbook.com（A记录）
ipv4.hscbook.com（AAAA记录）
ipv6.hscbook.com（AAAA记录）

配置脚本
在 CloudFlare 域名主页的最下面 API 处
将 Zone ID 填入脚本的 CFzoneid 变量
点击 Get your API token 获取 API token 填入脚本的 CFtkn 变量
将 CloudFlare 的邮箱账号填入 CFemail 变量
根据三项信息套入 
使用 cURL 获取 DNS Record ID
curl -X GET "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/dns_records" \
     -H "Content-Type: application/json" \
     -H "X-Auth-Key:$API_KEY" \
     -H "X-Auth-Email:$EMAIL"
并在 linux 终端中运行可取得子域名的 CFid 并填入 CFdomainid 变量
其他变量根据注释以实际情况自行修改后点击 Run Script 运行脚本测试，查看系统日志无报错即可
创建任务计划
修改好脚本后使用 WinBox 客户端连接至 RouterOS ；依次 System –> Scheduler进入任务计划列表新建一个任务计划间隔时间建议为 TTL 变量的两倍，内容为 /system script run "DDNS_CloudFlare";
END
参考文档：MikroTik Wiki
原脚本：Automatic script for Mikrotik RouterOS updating record on CloudFlare.
获取 CFid 可使用 API 调试工具，例：Postwoman(ApiDebug)

2、修改完毕，把script更新到ros需要添加如下执行权限：

*Read

*Write

*Test

*Sniff

*Sensitive

3、在ROS的system/scheduler添加一个轮训的计划任务：比如每五分钟执行一次

至此脚本全部设置完毕，看看是否更新。
