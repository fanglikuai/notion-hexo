---
categories: 整理输出
tags:
  - 缓存
sticky: ''
description: ''
permalink: ''
title: Redis 最佳使用实践
date: '2025-09-16 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/43539cac-2a74-4e44-9693-03381b35e458/106449882_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663GI5XFLA%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T010049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAqe7Blqkf%2FQFFUqL5p0J7MQNFk3am2P34Fa8lxHJPJNAiBCV1SsRIX%2F2zHnRtc%2BrfSWzaKLMWBT7NCTDNjy0WAwqCr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMFdHZR7p065iy%2BleeKtwDQFp8kLWSZW8IdYsxxTzR6zKpgNfDOngqea%2Fwt7bH9zAwl3Q64Z67TKLI8TL%2BR859D6v9jZmRK1qdKb9mAj1JS0YBi455%2FinfksXlQ0yJVABJ%2BBwY4%2FJTfLNhifEQuyZaBxpYe9bXX9Uk4pNiI2qSTAhL%2BSXRYT3HplPlx7gYlD8NOHM3tNXmioEfU2OeK3G87nkRqFhxPOVKZCc%2BrX8SqrFjKPFEtKhMzVq7W0UNv8tf4fGmHk0xeX4UU2W7n9kPkcx4il9bCaD1MHzxRYp0V2KkpaPJ%2B%2FYTrwv1xu%2BpM59TkMWjvlkDbhUZPO3fvlqJsWQ7qO%2FzOMmKOsYDUiR%2Fbi78Kr3n8UbeYLWn8iluRB5NzvmKfUIzuHAhVev4s30PpOUL%2Fi6nL4RlzjfmGgYxI6y96uDsYwlGRfo3QG4EIXOJWUMSMnYmCfjYYOWpRsnR0dgV6CWeMbi7m37bx8wxOdW9dCW%2F6aZzjsNrfvU%2BhWJnb41Av6W6tNlPUBM%2FbWdQb347AfQ%2Fz2i6%2BB8kOeiW9atMGoMxcBhXuROjfhi5hOP7c5x4lJ%2FiqylwcG4XsfZOHcmHY%2F%2FGYnKtxurYdGBdu2uSwS4QxvdOLT6UQzV5KzFcfeEOTO0hRmHvzR0whteTyQY6pgE9mEFC3N5xAhaa0P0dpLLXJ76%2Bf1DginoOfTQS29kDAHHw45QaRFFfHF61shWwyKhFkYI9tYW2As67Dbto9NR5iIyab9wQL81TnjG9QM9q4DILyEizuHNTaRIw%2BZMfI6JQBRTpMtWxz1gGqYFULnR%2BSClVyb%2B9B0lLNywFkkqhwS5qWv7PadmzhqMpyIDoEITScVrqR%2FpXNaIMMiHZRK8kSLXI2xh6&X-Amz-Signature=96538fd40261ef251ea939b32570d20df4cb3b12c15f0445ce43c37712d813b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

