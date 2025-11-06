---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STO5A26N%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T030051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDreU6rZDcYwh6fO4EbjCALdv6opjr2rB7DRieNItuOHwIhALbYt5pU334QSxWzD%2BUN8D%2FTYHpx0YJ0lbLSQl%2FgKYM5KogECJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwurSWkoDqcvV5PI98q3ANlaL1PlUvWttnGz6GynX4BmlIOyaG%2FnX0IxMkPsYvfW6w3ht56W7XX4O68FAutcWvB98K1G91hTCGtcCJ8sRxxAqOYtpnVfBDjH9hPwHXspmOA4tjD0fr9Fn8NEKYTy6NDB18aWfsePHQrj91zcM8y3J4Qeoyo45L4Phc0LQg%2FjUf%2FnI4ByE4sqOD9hft8DN9QchNXxQiA4Tty1TEFN4Paa32Q4zwaT43VoYTOJtzxZRNXE7aerd7uyfCJnKImbknGEy%2FQOFfWSsnRSGA06eBqlMs6R8HoRlC%2FA%2BJg%2Bd54VRvmMH0quSyuDlIpwsQuflSfeK%2FnpySSIcGWaMcRYswWLSKXq84W2cXky6rr%2BZDOhgt00PHSzS2b7%2FAWNRkQhwi1S9%2BGUA8p3VK4%2B%2FSNX7rfWYSKEwW8gyu5nQZT6E2fm2GR1x9e83CZiFesawVC1zEfpb7USY442Y%2BhLOUiClU8mCu%2B0V09ynirqC71u6Rzb49oMzzHl3GyNiQEmvTnfP1eEaCqGValttsS80VquEKlqCd72BQ0kqzZASBUne1OBSP2KW1OOytN1VRu%2B1qp4RMRcsOcfGEqt%2B6k%2BzzbJoxXvRaqIWOUZJILdK2LSjwb8cPvJgtAzzKyNPjhtTDN%2F6%2FIBjqkAdIH8eg3fgEZDY%2Fus0HhhdFYX7giRXPuMlouZqyzdkAeHKQh3pfBrm5WfxIFyPkqjcV1VnrH%2B8KCd26Om49QYGkBbgCvT9xW%2BNGVjwVIEo2G2dS%2BI9P%2F3hdxNXEjTudqvi6RucbONHEaLQAx37bksHnjyUeyn56bqzLy0HoXktijMihwR8z6oqaTlnivPQnaJ%2B1QRLIiZzVcNYhDSVREFIUKB5iL&X-Amz-Signature=64d868ac7253eda8341696ea3f19a2260a07bf52b44ee639360a09f066d02c98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 23:53:00'
index_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
banner_img: /images/4f27264a7e8afe769a5c2813552aa0f8.png
---

# bigkey 问题


![1753077336565-23eda3f0-dd0d-4865-9f4e-b536a19e7c9b.png](/images/c6758344cbe13f3ebf0f8718f40ab3f3.png)

- 使用离线库：将 Redis 所有数据导入 MySQL 然后进行查询
- redis-bigkey 命令`redis-cli -h 10.66.64.84 -p 10229 -a xxxx --bigkeys`
- rdb 文件扫描
- 生成 rdb，转成 csv 进行分析

删除：


底层介绍：

1. redis4以上，默认使用unlink命令
2. redis4以下，string直接del，其他类型如hash分批删除子元素，最后删除key

# 大key进行拆分


采用经典算法“分治法”，将大而化小。针对String和集合类型的Key，可以采用如下方式：

- String类型的大Key：可以尝试将对象分拆成几个Key-Value， 使用MGET或者多个GET组成的pipeline获取值，分拆单次操作的压力，对于集群来说可以将操作压力平摊到多个分片上，降低对单个分片的影响。
- 集合类型的大Key，并且需要整存整取要在设计上严格禁止这种场景的出现，如无法拆分，有效的方法是将该大Key从JIMDB去除，单独放到其他存储介质上。
- 集合类型的大Key，每次只需操作部分元素：将集合类型中的元素分拆。以Hash类型为例，可以在客户端定义一个分拆Key的数量N，每次对HGET和HSET操作的field计算哈希值并取模N，确定该field落在哪个Key上。

### 缺点


本质就是取模，需要在客户端进行操作，限定取模的数量，不够灵活

